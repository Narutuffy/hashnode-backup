## Microservices for startups, part 2


## The how-to guide to microservices for startups, small teams and solo developers

You may have been told that microservices and startups don’t go together. You might think that microservices are too difficult a starting point for a new product.

This is the typical wisdom, but I’d like to challenge it. With modern tools, the latest industry practices, and cheap and convenient cloud infrastructure (plus some cheating), it’s easier than ever before to start with microservices over starting with a monolith.

The simple advice is to start with a monolith first and then later refactor to microservices. But refactoring to microservices is a difficult and painstaking process. If you anticipate a need for microservices eventually, why not just go microservices-first from the start and save yourself the time-consuming and painful conversion?

Startups can use microservices. And it’s not as difficult or as complicated as you might have thought.

*Should* a startup use microservices? The default answer is *it depends*, promptly followed by a cautionary *probably not*.

But there are many startups and products that can definitely benefit from jumping straight into the deep end with microservices! These days, it’s easier to build with microservices than ever before, and they can be a good option for startups operating with limited resources.

In this blog post, we’ll look at ways that microservices can help startups and examine some shortcuts to help get you started quickly and cheaply.

# Why use microservices?

Scalability is the most obvious benefit to using microservices. Microservices applications are scalable to greater performance and capacity, and this is ideal for supporting a growing customer base.

They are also scalable from small to large organizations: as our business grows, we can grow our team around our application. Indeed, we can grow multiple teams around a growing microservices application.

Startups don’t need scalability in the early days. Of course, every startup dreams of scaling up big, and we hope to be able to do that, but microservices offer many other benefits that kick in long before we scale up:

# Automation

Microservices applications typically make extensive use of automation. This can scale the productivity of small teams, helping them to operate with a lower cost at a higher level and produce consistent, reliable results.

# DevOps culture

The DevOps culture promotes ownership within the development team, and typically, developers own the product all the way through to production. This removes the need for a separate operations team and can really empower a small team!

# Modern tools

The suite of modern tools (e.g., Docker, Terraform, Kubernetes, etc.) is there for the taking. These tools are well documented, well supported, and straightforward to learn. (OK, so Kubernetes is deep, but you might be surprised at just how quickly you can learn the fundamentals of it.) There is no need to build our own tools; we can and should build on the shoulders of giants.

# Cloud infrastructure

We have cheap and convenient access to cloud infrastructure. It has never been so easy as it is now to create the infrastructure for our application (e.g. you can create a Kubernetes cluster on Azure with a couple of clicks).

# Fine-grained

The fine-grained nature of microservices gives us granular control over performance, deployments, risk, and anything else you can think of relating to our application.

# Flexibility and extensibility

We can continuously extend and upgrade our application with minimal impact to our customer.

# Fault isolation

Problems in one part of the application don’t bring down the entire application; it can run in a degraded state and continue to be useful to our customers.

# Limitation of technical debt

As a startup, we have to cut some corners. It’s helpful to understand that the code in any particular microservice doesn’t really matter. Each microservice is so small that it can be thrown out and rewritten in a matter of days. Cheap and dirty implementations are officially OK, and we can easily replace them later.

These benefits add up to a continuous rapid pace of development. Rapid experimentation is so important to startups — we are trying to figure out exactly what it is that makes our product valuable!

Microservices help us reduce the cost of experimentation. We are better able to change things quickly and deploy reliably to get customer feedback. We can more quickly triangulate our product’s feature set.

# Before using microservices

Before you can decide whether microservices are right for your startup, there are a few things you need to consider:

1. Do we have the technical expertise and development capability for microservices? Some education is necessary. You need to know the tools, and you need to understand patterns for distributed applications. Learn the fundamentals first (see my book Bootstrapping Microservices) and then use just-in-time learning to fill in the deeper knowledge that is specific for your application as you develop it.
2. Validate your business model first. This almost goes without saying — with or without microservices. Before investing in the development of your application or infrastructure, you must be satisfied that your business model (or some variant of it) will work!
3. Can your product actually benefit from microservices? Do you need custom, scalable technology? You definitely don’t want to use microservices if you can imagine something simpler (e.g., WordPress) doing the job instead! Make sure you understand the benefits of microservices and how they intersect with the technical requirements of your product. If your product doesn’t really benefit from microservices, then you’ll end up paying the price without getting the reward.

Ultimately, the question is: Do I need to scale? 

Of course every startup wants to scale, but that doesn’t mean you need custom, scalable technology to achieve it. If it’s not clear that you need microservices, then you should instead prototype with simpler technologies until you outgrow them.

# Getting started with microservices for startups

Getting started with microservices doesn’t have to be difficult, although it can seem that way if you are learning and facing down a steep learning curve. That’s why education in the fundamentals is important. It’s only with some hindsight that you can see microservices aren’t as difficult as they seemed at the start.

When you are getting started developing a new product, there are many ways you can cut corners with microservices that will make starting out cheaper, simpler, and more convenient. Let’s get into some key techniques to make microservices more accessible in your early days as a startup.

## Use industry-standard tools

Don’t roll your own unless that’s the only way. Use a single repo and a single deployment pipeline as well. Keep your whole application (all your microservices) together in a single repository and use a single CD pipeline for deployment.

## Host your application in a single Kubernetes cluster

This drastically simplifies your security model; authenticate at the gateway, and treat internal connections as trusted. Run the cluster on a single virtual machine (VM). Until you need the performance or reliability, you don’t need more than one VM.

## No replicas

Until you need redundancy or performance, there is no need to make replicas of any of your microservices.

## Have a single database server

You should follow the best practice of having a separate database for each microservice, but you can host these databases on a single database server. There is no need to maintain a separate database server for each microservice!

## Do manual testing

In the earliest days, there is no need for automated testing. Instead, put your effort into building a script that can efficiently boot the application to a testable state (complete with realistic database fixtures) as quickly as possible. You want to make manual testing easy and efficient.

You’ll need automated testing eventually; it’s not possible to manage a large microservices application without automated testing. But it takes significant effort to stay on top of automated testing, and any work you do on such infrastructure detracts from your efforts to rapidly experiment with features that can make or break your entire product.

Do manual testing in the early days, then upgrade to automated testing as manual testing becomes less feasible.

## Automate deployments

Go to production early (you need this to get your application into the hands of customers) and have an automated CD pipeline in place from day one of going to production. In many ways, this is the most important part of your whole application! Getting feedback from customers (so we build the right product) is the lifeblood of a startup.

Using a single code repository and single deployment pipeline makes development with microservices much easier to manage. This somewhat defeats the point of microservices — having independently deployable services — but this doesn’t really matter in the early days of the startup!

Later on, it’s not difficult to extract your microservices to separate code repositories, each with its own independent deployment pipeline. When it comes time to do this, use the meta tool to have independent repositories as a single meta repository and continue to work on your application like it was a single repository.

# Pathways to scalability

Follow this advice when getting started with microservices and you’ll find that it’s a simpler and cheaper entrypoint for development with microservices.

Eventually, you’ll need to grow beyond cutting corners; you’ll need to scale up. And when you do, you’ll be glad that your foundation is scalable microservices with all the benefits that brings. Microservices offer many flexible pathways to scalability to ensure that your application can grow alongside your business.

I said at the start that prevailing wisdom presents a choice between a monolith and microservices. But I no longer think that it’s a simple binary choice of one versus the other. It’s more like there’s a sliding scale of possibilities from one large service through to many tiny services.

Monoliths and microservices are actually at opposite ends of the same spectrum, and in between them there’s an infinite variety of combinations, with services of varying sizes. You don’t have to decide simply between a monolith or microservices. You can, in fact, choose any point on that spectrum.

Know your options and make a choice that’s right for your company.