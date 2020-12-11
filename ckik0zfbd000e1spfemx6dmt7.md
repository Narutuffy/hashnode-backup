## Event Sourcing with Lagom on GCP

[Lagom](https://www.lightbend.com/lagom-framework) is a (very) opinionated microservice framework made by Lightbend (creators of Scala, Akka, Play more). Lagom is built on top of Akka and Play, supercharging it with an amazing engine to provide great performance. For those of you who still does not know Akka or Play I highly recommend to search it and look further. Two frameworks which I find very useful and reliable.

Lagom helps you build reactive systems, not just microservices, providing elasticity and resiliency out-of-the-box. Building Reactive Systems can be hard, but Lagom abstracts the complexities away. Akka and Play do the heavy lifting so you can focus on a simpler event-driven programming model on top, while benefiting from a message-driven system under the hood.

#### Event Sourcing

A paradigm shift of how we used to model our application’s state. Usually when something changes the state, we update the it and save it back to the database. In event sourcing instead of storing the state, you have to store the events (obviously…) which manipulates the application state. Using this event log we can later on reconstruct our state when needed. It is also easier to share events between services, keep everything immutable and change the data models whenever we want.

[Martin Fowler has a great explanation if you would like to read further](https://martinfowler.com/eaaDev/EventSourcing.html).

#### CQRS (Command Query Responsibility Segregation)

The core concept is to decouple the write model and read model. Usually we have one model which is used for everything but in CQRS we use two different models, one for inserting or updating data and the other for querying. It may be highly valuable to some cases and a total overhead to others. In my experience when your system evolves you will end up doing some kind of a CQRS.

[Again, Marting Fowler has an amazing explanation](https://martinfowler.com/bliki/CQRS.html).Lagom promotes these two concepts and make it ridiculously easy to build an Event Sourcing &amp; CQRS systems. At the heart of every service there are entities which react to commands. Each command persists one or more events and each event leads to the entity’s state change. We usually use HTTP calls or a message queue of some sorts to interact and send commands to these entities . To see a Lagom entity in action:

[lagom/online-auction-scala](https://github.com/lagom/online-auction-scala/blob/master/item-impl/src/main/scala/com/example/auction/item/impl/ItemEntity.scala)



But what this has to do with GCP? Up until not so long ago, it had nothing to do with GCP but recently two major milestones were achieved.

The first one is the release of [Lightbend Orchestration](https://developer.lightbend.com/docs/lightbend-orchestration-kubernetes/latest/index.html) which ease and automates the deployment of Lightbend based systems, Lagom fortunately is one them. As a GCP customer, you probably love Kubernetes and lucky enough, Kubernetes is a first class citizen in Lightbend Orchestration. It includes out-of-the-box service discovery based on the DNS mechanism of Kubernetes, automatic bootstrapping of Akka clusters and it even generates all the YAMLs needed for deploying your services to Kubernetes. Up until now we had to do everything manually and let just say it is not a walk in the park.

The second milestone is the support of Google Pub/Sub as a message broker for Lagom services instead of Kafka (discolsure the support provided by a module written by me and not by Lagom team). Lagom has an amazing abstraction of the message broker API so you can basically migrate from Kafka to Pub/Sub and backwards by changing the class your service inherit from. The communication using a message broker decouples the services making them transparent to each other and improves the system resiliency.

I am using Lagom for over half a year now, been tackling very complicated issues and I am very happy with this choice. It makes everything much easier to develop and test. The community is still quite small but very active and helpful.

I encourage you to give Lagom a shot and let me know how it goes, I am here to help :)
