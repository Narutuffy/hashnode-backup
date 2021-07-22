## Temporal - the iPhone of System Design

Temporal - the iPhone of System Design
======================================

Temporal ties Orchestration, Event Sourcing, and Workflows-as-Code in one distributed system and it is eating the world. #temporal #work #reflections

> Read time: 18 minutes | Published: Jul 19 2021

I'm excited to finally share why I've joined [Temporal.io](http://temporal.io/) as Head of Developer Experience. It's taken me months to precisely pin down why I have been obsessed with Workflows in general and Temporal in particular.

It boils down to 3 core opinions: Orchestration, Event Sourcing, and Workflows-as-Code.

_Target audience: product-focused developers who have some understanding of system design, but limited distributed systems experience and no familiarity with workflow engines_

[30 Second Pitch](https://swyx.io/#30-second-pitch)
---------------------------------------------------

The most valuable, mission-critical workloads in any software company are long-running and tie together multiple services.

*   **Because this work relies on unreliable networks and systems**:
    *   You want to standardize timeouts and retries.
    *   You want offer "reliability on rails" to every team.
*   **Because this work is so important**:
    *   You must never drop any work.
    *   You must log all progress.
*   **Because this work is complex**:
    *   You want to easily model dynamic asynchronous logic...
    *   ...and reuse, test, version and migrate it.

**Finally, you want all this to scale**. The same programming model going from small usecases to millions of users without re-platforming. Temporal is the best way to do all this — by writing idiomatic code known as **"workflows"**.

[Requirement 1: Orchestration](https://swyx.io/#requirement-1-orchestration)
----------------------------------------------------------------------------

Suppose you are executing some business logic that calls System A, then System B, and then System C. Easy enough right?

![Alt Text](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626948999199/oqechRc19.png)

But:

*   System B has rate limiting, so sometimes it fails right away and you're just expected to try again some time later.
*   System C goes down a lot — and when it does, it doesn't actively report a failure. Your program is perfectly happy to wait an infinite amount of time and never retry C.

You could deal with B by just looping until you get a successful response, but that ties up compute resources. Probably the better way is to persist the incomplete task in a database and set a cron job to periodically retry the call.

Dealing with C is similar, but with a twist. You still need B's code to retry the API call, but you also need another (shorter lived, independent) scheduler to place a reasonable timeout on C's execution time since it doesn't report failures when it goes down.

![Alt Text](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626949002930/EALmY4ieV.png)

Do this often enough and you soon realize that writing timeouts and retries are really standard production-grade requirements when crossing any system boundary, whether you are calling an external API or just a different service owned by your own team.

Instead of writing custom code for timeout and retries for every single service every time, is there a better way? Sure, we could centralize it!

![Alt Text](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626949006304/90kXjj9Te.png)

We have just rediscovered the need for **orchestration over choreography**. There are various names for the combined A-B-C system orchestration we are doing — depending who you ask, this is either called a Job Runner, Pipeline, or Workflow.

Honestly, what interests me (more than the deduplication of code) is **the deduplication of infrastructure**. The maintainer of each system **no longer has to provision** the additional infrastructure needed for this stateful, potentially long-running work. This drastically simplifies maintenance — you can shrink your systems down to as small as a single serverless function — and makes it easier to spin up new ones, with the retry and timeout standards you now expect from every production-grade service. Workflow orchestrators are "reliability on rails".

But there's a risk of course — you've just added a centralized dependency to every part of your distributed system. _What if it ALSO goes down?_

[Requirement 2: Event Sourcing](https://swyx.io/#requirement-2-event-sourcing)
------------------------------------------------------------------------------

The work that your code does is mission critical. What does that really mean?

*   **We cannot drop anything.** All requests to start work must either result in error or success - no "it was supposed to be running but got lost somewhere" mismatch in expectations.
*   **During execution, we must be able to resume from any downtime**. If any part of the system goes down, we must be able to pick up where we left off.
*   **We need the entire history** of _what_ happened _when_, for legal compliance, in case something went wrong, or if we want to analyze metadata across runs.

There are two ways to track all this state. The usual way starts with a simple task queue, and then adds logging:

    (async function workLoop() {
    	const nextTask = taskQueue.pop()
    	await logEvent('starting task:', nextTask.ID)
    	try {
    		await doWork(nextTask) // this could fail!
    	catch (err) {
    		await logEvent('reverting task:', nextTask.ID, err)
    		taskQueue.push(nextTask)
    	}
    	await logEvent('completed task:', nextTask.ID)
    	setTimeout(workLoop, 0)
    })()
    

But logs-as-afterthought has a bunch of problems.

*   The logging is not tightly paired with the queue updates. If it is possible for one to succeed but the other to fail, you either have unreliable logs or dropped work — unacceptable for mission critical work. This could also happen if the central work loop itself goes down while tasks are executing.
*   At the local level, you can fix this with batch transactions. Between systems, you can create two-phase commits. But this is a messy business and further bloats your business code with a ton of boilerplate — IF (a big if) you have the discipline to instrument every single state change in your code.

The alternative to logs-as-afterthought is logs-as-truth: If it wasn't logged, it didn't happen. This is also known as **Event Sourcing**. We can always reconstruct current state from an ever-growing list of `eventHistory`:

    (function workLoop() {
    	const nextTask = reconcile(eventHistory, workStateMachine)
    	doWorkAndLogHistory(nextTask, eventHistory) // transactional
    	setTimeout(workLoop, 0)
    })()
    

The next task is strictly determined by comparing the event history to a state machine (provided by the application developer). Work is either done and committed to history, or not at all.

I've handwaved away a lot of heavy lifting done by `reconcile` and `doWorkAndLogHistory`. But this solves a lot of problems:

*   Our logs are **always reliable**, since that is the _only_ way we determine what to do next.
*   We use **transactional guarantees** to ensure that work is either done and tracked, or not at all. There is no "limbo" state — at the worst case, we'd rather retry already-done work with idempotency keys than drop work.
*   Since there is no implicit state in the work loop, it can be **restarted easily** on any downtime (or scaled horizontally for high load).
*   Finally, with standardized logs in our event history, we can share **observability and debugging tooling** between users.

_You can also make an analogy to the difference between "filename version control" and git — Using event histories as your source of truth is comparable to a git repo that reflects all git commits to date._

But there's one last problem to deal with - how exactly should the developer specify the full state machine?

[Requirement 3: Workflows-as-Code](https://swyx.io/#requirement-3-workflows-as-code)
------------------------------------------------------------------------------------

The prototypical workflow state machine is a JSON or YAML file listing a sequence of steps. But this abuses configuration formats for expressing code. it doesn't take long before you start adding features like conditional branching, loops, and variables, until you have an underspecified Turing complete "domain specific language" hiding out in your JSON/YAML schema.

    [
      {
        "first_step": {
          "call": "http.get",
          "args": {
            "url": "https://www.example.com/callA"
          },
          "result": "first_result"
        }
      },
      {
        "where_to_jump": {
          "switch": [
            {
              "condition": "${first_result.body.SomeField < 10}",
              "next": "small"
            },
            {
              "condition": "${first_result.body.SomeField < 100}",
              "next": "medium"
            }
          ],
          "next": "large"
        }
      },
      {
        "small": {
          "call": "http.get",
          "args": {
            "url": "https://www.example.com/SmallFunc"
          },
          "next": "end"
        }
      },
      {
        "medium": {
          "call": "http.get",
          "args": {
            "url": "https://www.example.com/MediumFunc"
          },
          "next": "end"
        }
      },
      {
        "large": {
          "call": "http.get",
          "args": {
            "url": "https://www.example.com/LargeFunc"
          },
          "next": "end"
        }
      }
    ]
    

This example happens to be from [Google](https://github.com/GoogleCloudPlatform/workflows-samples/blob/main/src/step_conditional_jump.workflows.json), but you can compare similar config-driven syntaxes from [Argo](https://github.com/serverlessworkflow/specification/blob/57ed379acdf066c7dd87644b1ec2254f1f350ba6/comparisons/comparison-argo.md), [Amazon](https://states-language.net/spec.html), and [Airflow](https://airflow.apache.org/docs/apache-airflow/stable/tutorial.html#example-pipeline-definition). The bottom line is you ultimately find yourself hand-writing the Abstract Syntax Tree of something you can read much better in code anyway:

    async function dataPipeline() {
    	const { body: SomeField } = await httpGet("https://www.example.com/callA")
    	if (SomeField < 10) {
    		await httpGet("https://www.example.com/SmallFunc")
    	} else if (SomeField < 100) {
    		await httpGet("https://www.example.com/MediumFunc")
    	} else {
    		await httpGet("https://www.example.com/BigFunc")
    	}
    }
    

The benefit of using general purpose programming languages to define workflows — Workflows-as-Code — is that you get to **the full set of tooling** that is already available to you as a developer: from IDE autocomplete to linting to syntax highlighting to version control to ecosystem libraries and test frameworks. But perhaps the biggest benefit of all is the reduced need for context switching from your application language to the workflow language. (So much so that you could copy over code and get reliability guarantees with only minor modifications.)

This config-vs-code debate arises in multiple domains: You may have encountered this problem in AWS provisioning (CloudFormation vs CDK/Pulumi) or CI/CD (debugging giant YAML files for your builds). Since you can always write code to interpret any declarative JSON/YAML DSL, the code layer offers a superset of capabilities.

[The Challenge of DIY Solutions](https://swyx.io/#the-challenge-of-diy-solutions)
---------------------------------------------------------------------------------

So for our mission critical, long-running work, we've identified three requirements:

1.  We want an **orchestration** engine between services.
2.  We want to use **event sourcing** to track and resume system state.
3.  We want to write all this **with code** rather than config languages.

Respectively, these solve the pain points of reliability boilerplate, implementing observability/recovery, and modeling arbitrary business logic.

If you were to build this on your own:

*   You can find an orchestration engine off the shelf, though few have a strong open source backing.
*   You'd likely start with a logs-as-afterthought system, and accumulating inconsistencies over time until they are critical enough to warrant a rewrite to a homegrown event sourcing framework with stronger guarantees.
*   As you generalize your system for more use cases, you might start off using a JSON/YAML config language, because that is easy to parse. If it were entrenched and large enough, you might create an "as Code" layer just as AWS did with AWS CDK, causing an impedance mismatch until you rip out the underlying declarative layer.

Finally, you'd have to make your system scale for many users (horizontal scaling + load balancing + queueing + routing) and many developers (workload isolation + authentication + authorization + testing + code reuse).

[Temporal as the "iPhone solution"](https://swyx.io/#temporal-as-the-iphone-solution)
-------------------------------------------------------------------------------------

When [Steve Jobs introduced the iPhone](https://www.youtube.com/watch?v=MnrJzXM7a6o) in 2007, he introduced it as "a widescreen iPod with touch controls, a revolutionary mobile phone, and a breakthrough internet communications device", before stunning the audience: "These are _not_ three separate devices. This is **ONE** device."

<iframe src="https://www.youtube.com/embed/MnrJzXM7a6o" width="600" height="400"></iframe>

This is the potential of Temporal. Temporal has opinions on how to make each piece best-in-class, but the tight integration creates a programming paradigm that is ultimately greater than the sum of its parts:

*   You can build a UI that natively understands workflows as potentially infinitely long running business logic, exposing retry status, event history, and code input/outputs.
*   You can build workflow migration tooling that verifies that old-but-still-running workflows have been fully accounted for when migrating to new code.
*   You can add pluggable persistence so that you are agnostic to what databases or even what cloud you use, helping you be cloud-agnostic.
*   You can run polyglot teams — each team can work in their ideal language, and only care about serializable inputs/outputs when calling each other, since event history is language-agnostic.
*   There are more possibilities I can't talk about yet.

[The Business Case for Temporal](https://swyx.io/#the-business-case-for-temporal)
---------------------------------------------------------------------------------

A fun anecdote about how I got the job: through blogging.

While exploring the serverless ecosystem at Netlify and AWS, I always had the nagging feeling that it was incomplete and that the most valuable work was always "left as an exercise to the reader". The feeling crystallized when I rewatched DHH's 2005 Ruby on Rails demo and realized that there was no way the serverless ecosystem could match up to it. We broke up the monolith to scale it, but there were just too many pieces missing.

I started analyzing cloud computing from a "Jobs to Be Done" framework and wrote two throwaway blogposts called [Cloud Operating Systems and Reconstituting the Monolith](https://gist.github.com/sw-yx/ff8a4f6757286444fa20b43f6b98b205). My ignorant posting led to [an extended comment from a total internet stranger](https://twitter.com/swyx/status/1226747097208295424) telling me all the ways I was wrong. [Lenny Pruss](https://twitter.com/lennypruss?lang=en), who was ALSO reading my blogpost, saw this comment, and got Ryland to join Temporal as Head of Product, and he then turned around and pitched (literally pitched) me to join.

One blogpost, two jobs. [Learn in Public](https://www.swyx.io/LIP) continues to amaze me by the [luck it creates](https://www.swyx.io/create-luck).

Still, why would I quit a comfy, well-paying job at Amazon to work harder for less money at a startup like this?

*   **Extraordinary people**. At its core, betting on any startup is betting on the people. The two cofounders of Temporal have been working on variants of this problem for over a decade each at AWS, Microsoft, and Uber. They have attracted an extremely high caliber team around them, with centuries of distributed systems experience. I report to the Head of Product, who is one of the fastest scaling executives Sequoia has ever seen.
*   **Extraordinary adoption**. Because it reinvents service orchestration, Temporal (and its predecessor Cadence) is very horizontal by nature. [Descript uses it](https://docs.temporal.io/blog/descript-case-study) for audio transcription, [Snap uses it](https://eng.snap.com/build_a_reliable_system_in_a_microservices_world_at_snap) for ads reporting, [Hashicorp uses it](https://temporal.io/#final-quote) for infrastructure provisioning, [Stripe uses it](https://stripe.com/jobs/listing/infrastructure-engineer-developer-productivity-workflow-engine/2964407) for the workflow engine behind Stripe Capital and Billing, [Coinbase uses it](https://docs.temporal.io/blog/reliable-crypto-transactions-at-coinbase/) for cryptocurrency transactions, [Box uses it](https://docs.temporal.io/blog/Temporal-a-central-brain-for-Box) for file transfer, [Datadog uses it](https://www.youtube.com/watch?v=eWFpl-nzGsY) for CI/CD, [DoorDash uses it](https://doordash.engineering/2020/08/14/workflows-cadence-event-driven-processing/) for delivery creation, [Checkr uses it](https://docs.temporal.io/blog/how-temporal-simplified-checkr-workflows/) for background checks. Within each company, growth is viral; once one team sees successful adoption, dozens more follow suit within a year, all through word of mouth. ![Alt Text](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626949011287/WQ9hTS37Pi.png)
*   **Extraordinary results**. After migrating, Temporal users report production issues falling from once-a-week to near-zero. Accidental double-spends have been discovered and fixed, saving millions in cold hard cash. Teams report being able to move faster, thanks to testing, code reuse, and standardized reliability. While the value of this is hard to quantify, it is big enough that users organically tell their friends and [list Temporal in their job openings](https://temporal.io/careers#external-jobs).
*   **Huge potential market growth**. The main thing you bet on when it comes to Temporal is that its primary competition really is homegrown workflow systems, not other engines like Airflow, AWS Step Functions, and Camunda BPMN. In other words, even though Temporal should gain market share, **the real story is market growth**, driven by the growing microservices movement and developer education around best-in-class orchestration. At AWS and Netlify, I always felt like there was a missing capability in building serverless-first apps — duct-taping functions and cronjobs and databases to do async work — and it all fell into place the moment I saw Temporal. I'm betting that there are many, many people like me, and that I can help Temporal reach them.
*   **High potential value capture**. Apart from market share and market growth, any open source project has the additional challenge of value capture, since users can self-host at any time. I mostly subscribe to David Ulevitch's take that [open source SaaS is basically outsourcing ops](https://mobile.twitter.com/swyx/status/1373425786351284228). I haven't talked about [Temporal's underlying architecture](https://docs.temporal.io/blog/workflow-engine-principles) but it has quite a few moving parts and takes a lot of skill and system understanding to operate. For reasons I won't get into, Temporal scales best on Cassandra and that alone is enough to make most want to pay someone else to handle it.
*   **Great expansion opportunities**. Temporal is by nature the most direct source of truth on the most valuable, mission critical workflows of any company that adopts it. It can therefore develop the most mission critical dashboard and control panel. Any source of truth also becomes a natural aggregation point for integrations, leaving open the possibility of an internal or third party service marketplace. With the Signals and Queries features, Temporal easily gets data in and out of running workflows, making it an ideal foundation for the sort of human-in-the-loop work for [the API Economy](https://www.swyx.io/api-economy/). Imagine toggling just one line of code to A/B test vendors and APIs, or have Temporal learn while a domain expert manually executes decision processes and take over when it has seen enough. As a "high-code" specialist in reliable workflows, it could be a neutral arms dealer in the "low-code" gold rush, or choose to get into that game itself. If you want to get really wild, the secure distributed execution model of Workflow workers could be facilitated by an [ERC-20 token](https://www.investopedia.com/news/what-erc20-and-what-does-it-mean-ethereum/). (_to be clear... everything listed here is personal speculation and not the company roadmap)_

There is much work to do, though. Temporal Cloud needs a lot of automation and scaling before it becomes generally available. Temporal's UI is in the process of a full rewrite. Temporal's docs need a lot more work to fully explain such a complex system with many use cases. Temporal still doesn't have a production-ready Node.js or Python SDK. And much, much, more to do before Temporal's developer experience becomes accessible to the majority of developers.

If what I've laid out excites you, take a look at [our open positions](https://temporal.io/careers) (or write in your own!), and [join the mailing list](https://docs.temporal.io/docs/concepts/introduction#mc_embed_signup_scroll)!

[Further Reading](https://swyx.io/#further-reading)
---------------------------------------------------

*   Orchestration
    *   [Yan Cui's guide to Orchestration vs Choreography](https://theburningmonk.com/2020/08/choreography-vs-orchestration-in-the-land-of-serverless/)
    *   [InfoQ: Coupling Microservices](https://www.infoq.com/podcasts/design-time-coupling-microservices/) - a non-Temporal focused discussion of Orchestration
    *   [A Netflix Guide to Microservices](https://www.youtube.com/watch?v=CZ3wIuvmHeM)
*   Event Sourcing
    *   Martin Fowler on [Event Sourcing](https://martinfowler.com/eaaDev/EventSourcing.html)
    *   Kickstarter's guide to [Event Sourcing](https://kickstarter.engineering/event-sourcing-made-simple-4a2625113224?gi=e356daabcb81)
*   Code over Config
    *   [ACloudGuru's guide to Terraform, CloudFormation, and AWS CDK](https://acloudguru.com/blog/engineering/cloudformation-terraform-or-cdk-guide-to-iac-on-aws)
    *   [Serverless Workflow's comparison of Workflow specification formats](https://github.com/serverlessworkflow/specification/tree/main/comparisons)
*   Temporal
    *   [Dealing with failure](https://docs.temporal.io/blog/dealing-with-failure/) - when to use Workflows
    *   [The macro problem with microservices](https://stackoverflow.blog/2020/11/23/the-macro-problem-with-microservices/) - Temporal in context of microservices
    *   [Designing A Workflow Engine from First Principles](https://docs.temporal.io/blog/workflow-engine-principles/) - Temporal Architecture Principles
    *   [Writing your first workflow](https://www.youtube.com/watch?v=taKrIWt6KMY&feature=youtu.be) - 20min code video
    *   [Case studies](https://docs.temporal.io/blog/tags/case-study) and [External Resources](https://docs.temporal.io/docs/external-resources/) from our users

[← All Essays](http://swyx.io/ideas?show=Essays) [Featured Essays →](http://swyx.io/#featured-writing)

* * *

Join 2,000+ developers getting updates ✉️

Subscribe

Send a TL;DR of the top 10 swyx.io posts!

We won't send you spam. Unsubscribe at any time.

[Built with ConvertKit](https://convertkit.com/?utm_source=dynamic&utm_medium=referral&utm_campaign=poweredby&utm_content=form)

Too soon! [Show me what I'm signing up for!](https://tinyletter.com/swyx/archive)

* * *

### Webmentions

[](http://swyx.io/writing/clientside-webmentions)

[Tweet about this post](https://twitter.com/intent/tweet/?text=Great%20post%20by%20@swyx%20https://www.swyx.io/why-temporal) and it will show up here, or you could [leave a comment on Dev.to](https://dev.to/swyx/temporal-the-iphone-of-system-design-4a78)

Loading...