## How we migrated to serverless in a weekend?

One of the crucial elements in [Daily’s](https://daily.dev/) architecture is the post ingestion pipeline. The pipeline has to subscribe to various RSS and HTML feeds for new posts, crawl the post to fetch metadata information (image link, publish time, author and such) and lastly download, process and upload to CDN the post image. Everything of course runs in real-time in order to provide the newest articles the moment they are published.

Until recently this architecture was powered by an Akka Stream cluster running on preemptible machines to reduce cloud cost. The Akka cluster was in-charge of everything from discovering to sending the articles to central application server to store in the database. For those of you who don’t know Akka, it is a powerful JVM framework for concurrent applications and Akka Stream specifically let you build a data processing pipelines in a stream manner. I love to use it but I felt that this time it was an overkill because Akka is a stateful application which brings a lot of DevOps challenges compared to stateless applications. When it comes to scalability, you have to make sure that the cluster nodes can discover each other, monitoring is much harder and more.

This situation was a call for simplicity, I decided to refactor everything but this time I have to separate concerns, build stateless workers and monitor everything so I can sleep tight.

### Separating Concerns

First thing first, let’s separate concerns which is pretty easy for our use case:

*   Post discovery — Subscribe to RSS or other feeds and look for new posts.
*   Post enrichment — Enrich the basic information received from the feed with much richer information such as author, keywords, image and more.
*   Image processing — Download the post image, scale it to relevant size, create a placeholder and upload everything to CDN

### Stateless &amp; Decoupling

Second, I want that the different workers will be stateless and decoupled from each other. Each worker has a single task to complete and nothing more.

Enter Cloud Pub/Sub, I decided to use Cloud Pub/Sub to deliver messages between the workers as it is a managed message queue provided by Google Cloud. Obviously, scalability is not an issue with a managed service and also the workers don’t have to know anyone besides the Pub/Sub which is in-charge of providing the next task and also to receive the result (which is other worker task). Each worker simply add more information to the post until it is ready and the central application server adds it to the database.

### Make It Happen

Let’s get the **** done and built it already! At the time, Google Cloud Functions support for nodejs 6 was in Beta which in Google standards this is mostly like GA so I decided to give it a go as it requires me to build everything stateless and it auto scales the workers based on Cloud Pub/Sub load.

For the post discovery, I found a service called [Superfeedr](https://superfeedr.com) which does exactly this, all I need is to build a HTTP webhook which is notified by Superfeedr and sends a message to Pub/Sub.

It took me only one weekend to write and deploy everything. I ended up with three projects with approximately 100 lines of code each including Cloud Function boilerplate code. The system now is very flexible as I can deploy each function separately, manage its own dependency, Pub/Sub makes everything fault tolerant and more.

One significant drawback I currently experience with Cloud Functions is that there is no deployment strategy (blue/green, rolling update, etc…) but for my use case it is not as important as the other benefits.

### Monitoring

To sleep tight at night I must set appropriate monitoring and alerting so I can be sure everything is working fine and our users receive the latest news in real-time.

I use Stackdriver as my monitoring platform. I added Stackdriver Trace to the Cloud Functions to make sure I can see the errors in a central dashboard and set alerting rules. In addition, I set alerting based on the ratio of unacknowledged messages in Pub/Sub meaning one of my services doesn’t work and need a special care. Lastly, I defined custom logging metrics to track down errors more easily.

Now when something bad happen I get notified in Slack immediately and I can fix it as soon as possible.This is our migration story which was pretty quick I think, most of it was done in one weekend. The monitoring was added later and of course bug fixes were done along the way. The final result is a resilient and elastic system which delivers real-time posts to Daily.
