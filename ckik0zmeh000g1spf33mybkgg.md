## My Microservices Stack

Here is my some what opinionated stack for building resilient and elastic distributed system. I describe each framework or platform with a single marketing line followed by how I use it.

*   [Node.js](https://nodejs.org/) — A Javascript runtime built on [Chrome’s V8 JavaScript engine](https://developers.google.com/v8/). Used to build stateless API and automation scripts. Usually my “go-to” language for everything unless heavy lifting is needed.
*   [Lagom](https://www.lagomframework.com/) — An open source framework for building reactive systems in Java or Scala. Used to build systems and API based on Event Sourcing &amp; CQRS.
*   [Akka](https://akka.io/) — An open source toolkit for building highly concurrent, distributed and resilient message-driven applications for Java and Scala. Used to build data processing and complex processed based on the [Actor Model](https://doc.akka.io/docs/akka/current/guide/actors-motivation.html?_ga=2.214845855.964496164.1531660745-1103146012.1531660745).
*   [PostgreSQL](https://www.postgresql.org/) — World’s most advanced open source relational database. Usually my “go-to” database as it fits most of the use cases and most cloud services provide it as a service.
*   [Docker](https://www.docker.com/) — An open platform build, ship, and run distributed applications, whether on laptops, data center VMs, or the cloud. Used to package the applications in a uniform and immutable regardless the language or framework inside.
*   [Kubernetes](https://kubernetes.io/) — An open-source system for automating deployment, scaling, and management of containerized applications. Nothing more to say, just the perfect solution for deployment and application orchestration.
*   [Google Cloud Platform](https://cloud.google.com/) — Build, innovate, and scale. It simply provides everything I need for running my applications. Kuberenetes, Postgres, Docker registry and much more as a service with no ops needed (just your credit card, of course :P)

Looking forward to see your stack and comments!
