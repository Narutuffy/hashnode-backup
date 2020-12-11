## The Pursuit of Perfect Pipeline: Part #4 Continuous Delivery

It is now the time to finally talk about the last piece in the puzzle. [TDD](/posts/pursuit-of-perfect-pipeline-part-1/), [branching model](/posts/pursuit-of-perfect-pipeline-part-2/) and [CI](/posts/pursuit-of-perfect-pipeline-part-3/) should help you build better products, so now the only thing left to be done is to ship them quickly and easily. In this blog post I will focus on delivering cloud oriented projects (webapps, servers, microservices, etc). I also plan to write a post about CD for mobile apps but it will take a while.

### What is it anyway?

Martin Fowler describes it best in his article about [Continuous Delivery](https://martinfowler.com/bliki/ContinuousDelivery.html):

_Continuous Delivery is a software development discipline where you build software in such a way that the software can be released to production at any time._

_You’re doing continuous delivery when:_

*   _Your software is deployable throughout its lifecycle_
*   _Your team prioritizes keeping the software deployable over working on new features_
*   _Anybody can get fast, automated feedback on the production readiness of their systems any time somebody makes a change to them_
*   _You can perform push-button deployments of any version of the software to any environment on demand_

I recommend reading the whole article, as it gives a lot of background about CD, I will try to provide the tools.

Let’s focus for a moment on the list above of the prerequisites for CD. The first two criteria are being taken care by our branching model which keeps the master branch deployable at any given time. The third criterion is done by Jenkins (or any other CI platform), which runs the tests on each commit pushed to the repository. The fourth criterion is the focus of this post as we haven’t solved it yet.

### Spinnaker as the best choice around

There are many ways to achieve “push-button deployments” (bash scripts, Jenkins job, CD as a service, etc) but I still haven’t find a complete solution as [Spinnaker](http://www.spinnaker.io/) for cloud oriented projects. It has almost became the “Jenkins” of CD. It has many integrations to different cloud providers &amp; on-premise solution (AWS, GCP, Azure, Kubernetes, etc), triggers, notification services and much more. The only downside compared to Jenkins is that it is not as “plugin-able” as Jenkins. Right now it is not possible to build plugins to Spinnaker, one must dive into the source code and create a PR.

Spinnaker can provision clusters, load balancers, monitor the health of a deployment and execute a deployment pipeline. Spinnaker follows and enforces one of the best deployment practices, immutable infrastructure. You can not change anything in your deployment after it has been deployed, not even a single environment variable. It has already been proved that “tweaking” a deployed infrastructure is an error prone process and can also affect future deployments. Spinnaker provisions a new cluster for every new version or a change of configuration and just sets the load balancer accordingly.

Similar to Jenkins, I prefer to deploy Spinnaker to Kubernetes but you can also use regular VMs. The Spinnaker team is working on a tool called [Halyard](https://github.com/spinnaker/halyard), it is in Beta stage right now but I highly recommend to use it. It makes the deployment a piece of cake and the developers are very active and responsive in Spinnaker’s Slack organization, so you can ask any question you may have.

### Deployment Pipeline

After we chose the right platform to orchestrate and execute this whole operation, we have to decide exactly what exactly are the deployment stages. Let’s assume that you have only staging and production environment for the simplicity of the example.

We have to decide first what will trigger the pipeline, I prefer to use a docker registry trigger but there are more options. It means that each time a new docker image version is pushed, Spinnaker will start running the pipeline. Usually Jenkins is in charge of building these docker images.

Next we have to provision a new server group in the staging environment, it is important to set the environment variables when defining the server group and also set the readiness and healthiness probes, so Spinnaker will be able to validate the deployment. For staging environment we don’t need any fancy deployment strategy, so after Spinnaker validates that the server group is up, we can proceed to deleting the old server group.

Usually after deploying to staging, you would want to run end to end tests or even just acceptance tests to validate that everything works as it should, this can be done by triggering a Jenkins job or running a bash script.

Deploying to production besides being a technical matter is also a business matter, so advise your product manager how often you should deploy and what kind of validations each version should go through.

Usually before promoting to production, you would like to do some manual tests, so you can a manual judgment stage which halts the pipeline until someone approves it.

Now we have to think about what is the best deployment strategy that fits the product. The main two strategies are red/black (blue/green) and rolling push.

Red/black will create a new server group similar to the existing one and when the server group is healthy, Spinnaker will smoothly transition the traffic from one to another.

Rolling push will replace each instance of the existing server group with a new instance running the new version. Spinnaker will wait for the instance to be healthy and only then will proceed to the next one.

Of course, both strategies support rollback in case of failure. There are pros and cons for each strategy, just choose what suits you best.

When deployment is done and the new server group is healthy it’s a good time to run smoke tests and acceptance tests to validate that the server group is functioning well in production. We all know and hate those “production-only” bugs so it is not enough to rely on the tests that ran in staging.

When we sure everything is good and stable, we can tell Spinnaker to disable (or even delete, not recommended) the previous server group.

If you need to do database migrations or any setup steps be sure to automate them, attach the scripts to the repository and run them during the deployment pipeline.

Viola! Now our development pipeline is in line with all Martin Fowler’s CD criteria. We can deploy any commit from master branch to any environment in a click of a button. The step from here to continuous deployment, when every commit is being deployed automatically without manual approval is just a business decision that has to be taken, technically speaking you are ready.

So that’s it, this is the end of the series. I hope that these blog posts will help build faster and better products and of course sleep more at night. Let me know how it goes!
