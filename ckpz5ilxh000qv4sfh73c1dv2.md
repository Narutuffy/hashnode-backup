## Microservices for startups, part 1


## The Sortal architecture and why we choose microservices for our MVP

You may have been told that development with [microservices](https://en.wikipedia.org/wiki/Microservices) is complicated or difficult. Certainly, as startups, we are told that we shouldn’t start with microservices. Instead, we are told to start with a [monolith](https://en.wikipedia.org/wiki/Monolithic_application) and restructure later to microservices.

In this blog post, I’ll challenge that conventional wisdom. I don’t believe that microservices are just for the goliaths of the software industry. I believe that startups, small teams, and solo developers can also make effective use of microservices and that they are not as difficult as you might have thought.

My startup, Sortal, approached development with a microservices-first approach. It made a lot of sense for us to build our product this way. We found ways to keep costs low and were still able to benefit from everything offered by microservices.

When you think of microservices you might automatically think of humongous products like Netflix. A microservices architecture obviously helps manage incredibly complex applications like this and massive scale is probably the most well known benefit of microservices. However microservices aren’t just for hugely complex applications. 

Did you know that you can also use microservices at a much smaller scale? The fact is that all applications start out small and this is even true of microservices. Like any development process we can start small and then iterate towards greater complexity (illustrated in Figure 1). This is how big applications are built. They never start out at such a monstrous size!

![Start simple](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828286771/tkB9Lgm-NX.png)
##### Figure 1: Applications always start out small and simple (even microservices applications!). We grow a large and complex application through a sequence of many small and simple iterations.

Please don’t get me wrong, to grow a big application you need to invest in your team, your infrastructure and substantial automation, you can’t get away from that. However in the early days of the startup we can cut some corners and simplify the development process. This makes the microservices-first approach significantly more appealing on a small scale. 

My work building microservices for startups led to my latest mission: making microservices more accessible. Part of this effort has been writing down my years of experience in my new book [Bootstrapping Microservices](http://bit.ly/2o0aDsP) (published by Manning). This is a practical and project based guide to building applications with microservices to help you navigate this complex learning landscape. There are many theoretical books on microservices, but this is the only one that I know of that is thoroughly practical. It’s like a guided tour through learning how to build your own microservices application.

In this blog post we’ll open up the Sortal architecture and explain why we used microservices and the benefits they convey to our customers. 

![Bootstrapping Microservices book](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828287479/5YqBZf7cqk.png)

# Sortal

Our product, Sortal, was built microservices-first. We did an initial mobile prototype to help validate the idea. After that, making the move to the web, we decided to jump directly to microservices and skipped the more usual monolithic-stage. 

Our product is tech-heavy with many moving parts and microservices seemed like the best  way to proceed. Along the way though we discovered many other benefits of working with microservices.

## What is Sortal?

Sortal is a SaaS product for digital asset management. It streamlines the way people engage with their digital media from a centralised, accessible content library. We believe it should be easy for teams to quickly navigate through thousands of digital ﬁles. 

Sortal’s core underlying feature is artificial intelligence. We use machine learning to identify the content of assets, identify relationships between them and learn from our users to customise their workflow and user experience. 

Please watch the video below for a short overview of Sortal.

<iframe width="560" height="315" src="https://www.youtube.com/embed/IOIMM4YPIdU" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Sortal's deployment pipeline

Sortal has a simple but effective deployment pipeline. Developers push code to a hosted code repository on Bitbucket. 

On each code push the continuous delivery pipeline is invoked. This executes a handful of shell scripts that do the following:

- Run unit tests against code;
- Build and publish Docker images;
- Run integration tests against Docker images;
- Invokes Terraform to build our infrastructure on Microsoft Azure and;
- Publish our microservices to our Kubernetes cluster running on Azure. 

Our process is illustrated in figure 2.

![Sortal's deployment pipeline](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828286766/7TfdDHsfK.png)
##### Figure 2: The deployment pipeline for Sortal

We like to keep things simple, so we have only two branches in our code-base:

1. The master branch which automatically deploys to our Test cluster; and
2. The live branch which automatically deploys to our Live cluster.

There are many branching strategies we could use, but **for startups simplicity is the order of the day**. Besides that excessive use of branches makes [continuous integration](https://en.wikipedia.org/wiki/Continuous_integration) much more difficult.

General development is conducted in the master branch and changes are periodically merged to the live branch. Customer-facing issues and important hotfixes are often applied to live and merged back to master - this strategy means that we don’t have to foist unnecessary changes on the customer between official releases.

## Automation

Our automated deployment pipeline is a crucial piece of our product. Why is that? It’s because this is how we quickly and frequently deploy updates to our customers. We need feedback from customers so that we can improve our product. **Feedback is lifeblood for startups**.

To update our application we simply update our code, an approach known as [continuous delivery](https://en.wikipedia.org/wiki/Continuous_delivery). 

We also build and update our cloud infrastructure through code, this approach is called [infrastructure as code](https://en.wikipedia.org/wiki/Infrastructure_as_code).

Automated deployment and automated testing are essential for building applications with microservices. The fact that microservices are tiny (they are indeed micro sized) means that you typically need a lot of them to build an application. How many? Sortal has 17 and counting. Even though this is still a small application that’s still a lot of processes to manage, especially for a small team that is running lean. 

We couldn’t manage this without automation. I’d go as far as to say that microservices don’t work without automation. **If you are planning to use microservices make sure that automation is core to your technology strategy**.

Having an automated pipeline helps enable a fluid and fast pace of development. With reliable automation in place we automatically stop worrying about the scaffolding and start focusing on delivering features that add value for our customers.

## Cloud independence

Although we use Azure, most of Sortal is built on cloud independent technology. Indeed that’s why we use such cloud-agnostic technologies such as Docker, Terraform and Kubernetes. 

All of these tools and technologies are cloud vendor neutral:

- Docker allows us to package and publish our microservices independently of where they are deployed.
- Kubernetes is a computing platform that abstracts away the details of the underlying infrastructure.
- Terraform allows us to build infrastructure and deploy to any cloud. It supports this by provider plugins that allow it to talk to the different clouds. You can see this in figure 3.
- In addition we use RabbitMQ (an open source message queue) for our inter-microservice communication.

![Terraform's cloud independence](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828287123/Ojo7KHLqfE.png)
##### Figure 3: Terraform can target any of the major cloud vendors through plugin providers.

Cloud independence is important to me on principle, personally I like to avoid vendor lock in where possible, but  also think **it’s important as a startup that we keep our options open for as long as possible**. We are working hard to provide options for our customers and investors and we’d like to be able to redeploy Sortal wherever it can be of value.

Please note that I’m not saying it will be easy to change from one cloud platform to another, of course that’s going to take significant effort. All I’m saying is that it’s much more feasible because the application itself is not relying on anything that is cloud-vendor specific.

## Sortal architecture

Sortal is currently composed of 17 microservices (and counting) running on a Kubernetes cluster. Figure 4 is a simplified view of the architecture showing some of our most important microservices. 

![Sortal's microservices architecture](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828287169/-eC6_uo2E1.png)
##### Figure 4: The microservices-based architecture of Sortal.

The following table provides brief descriptions of each microservice.

<table>
    <tr>
        <th>
            Microservice
        </th>
        <th>
            Purpose
        </th>  
    </tr>
    
    <tr>
        <td>
            Gateway
        </td>
        <td>
            The gateway to the outside world. Allows customers to interact with Sortal through the web.<br>
            <br>
            Responsible for authentication.
        </td>
    </tr>

    <tr>
        <td>
            Asset upload
        </td>
        <td>
            Manages the upload of assets. Notifies the system that new assets have arrived.<br>
            <br>
            This is our most crucial microservice, because being able to reliably capture customer uploads is core to our functionality.<br>
            <br>
            Microservices are useful here because it allows us to keep this code small. That’s important because less code means there is less space for bugs to hide and less bugs mean we can more reliably capture uploaded assets.
        </td>
    </tr>

    <tr>
        <td>
            Asset storage
        </td>
        <td>
            An abstraction on our external file storage. Allows us to plugin in different storage engines depending on customer requirements (again keeping our options open).
        </td>
    </tr>

    <tr>
        <td>
            Job queue
        </td>
        <td>
            Manages jobs (i.e. thumbnail generation) that are applied to assets.<br>
            <br>
            When jobs fail or timeout they are automatically retried.
        </td>
    </tr>

    <tr>
        <td>
            Asset pipeline
        </td>
        <td>
            The asset pipeline comprises various microservices that implement the jobs (i.e. thumbnail generation) that are applied to assets to extract baseline information from assets.<br>
            <br>
            Metadata produced by the asset pipeline is published to the metadata service.
        </td>
    </tr>

    <tr>
        <td>
            Metadata
        </td>
        <td>
            Collects and maintains a database of the metadata for every asset.
        </td>
    </tr>

    <tr>
        <td>
            Learning engine
        </td>
        <td>
            The Sortal learning engine learns as customers use the product.<br>
            <br>
            It automatically builds an additional layer of meaning on top of the baseline information.<br>
            <br>
            It customises the Sortal user experience to be unique for each customer.
        </td>
    </tr>

    <tr>
        <td>
            Search
        </td>
        <td>
            Bakes the metadata into an efficient search index allowing customers to quickly search for relevant assets.
        </td>
    </tr>
</table>

# Why microservices?

We knew early on that microservices were the right fit for the web-based version of Sortal. We had prototyped the app in mobile form and found it to be complex, tech-heavy and with many moving parts. Microservices are simply fantastic for managing complex applications like this, but following are some of the other things we found to love about microservices.

## Storage independence

Note how figure 4 shows that we use external file storage. The asset storage microservice serves as an abstraction to the underlying file storage **giving us plenty of freedom on where we can store customer assets**. 

We can change our file storage engine by replacing the storage microservice. Due to the independent nature of microservices we can even make this change while the application is running without affecting customers who are using the platform.

## Fast development cycle

Having the ability to quickly evolve, update and extend any modern software is important, but **for startups such speed is critical**. We must move quickly and find the right product to market fit before we exhaust our runway.

One key to Sortal’s success is being able to continuously evolve it. The baseline machine learning underlying Sortal must be updated so that we can stay on top of the latest research and development in artificial intelligence.

Microservices support this by being independently updatable. We can update and add microservices without stopping or restarting the application and this enables a fast pace of development. Our customers continue using our platform while we are continuously upgrading it.

Kubernetes support for replicas and rolling updates even means we can update the gateway (the customer access point) potentially without disconnecting or interrupting our customers.

## Fault isolation

We are developing complex software and problems can’t always be avoided. Instead of trying to be perfect and stop problems from happening we use microservices to manage and contain problems and mitigate the risk to our customers.

Updates to microservices applications are inherently less risky. Each microservice is small so the surface area for problems is minimized and even those that do occur are more easily rolled back. In addition to that we typically do development through many small incremental changes. Small changes are easier to test, easier to troubleshoot and easier to fix when problems are discovered. 

**Microservices give us the power to isolate many problems before they affect our customers**. Our customers are buffered from the impact of problems that happen deep within our cluster. This gives us plenty of scope to detect and fix issues before they can affect our customers.  

A useful side effect of this isolation is that as developers we can also isolate technical debt within microservices. This is useful for startups who need to move quickly and prototype new features. 

We don’t have to care as much about the quality of the code, due to the small size of the microservice it’s easier to fix it later, even if it comes to throwing it out and starting over.

This gives you a useful sizing guide for microservices. **Each microservice should be replaceable within days or at the most weeks**. Any bigger than that and you can’t really consider it ‘micro’.

## Scalability

Of course a major benefit of microservices is scalability. Startups don’t need this immediately, we are starting small, proving our product and building our customer-base. 

However **it is the dream of every startup to be able to scale** and microservices give us many knobs we can turn to rapidly increase the performance and capacity of our system.

Using microservices gives us granular and independent control over the performance of the application. This was important to us because we knew that some of our services would be extremely compute intensive. Using microservices allowed us to separate these from the gateway so there is no chance that a heavy weight process can impinge on the user experience of our customers. Delivering a good user experience is very important to us and performance is a part of that equation.

We can scale the performance heavy parts of the system independently to increase their capacity. We can even do this elastically, scaling up to meet peak demand and then scaling down afterward. **This allows us to fine tune our rate of asset processing to control the trade off between performance and cost**. Managing costs is important to startups who are operating lean.

Microservices applications are also scalable for development teams. As the application grows larger and more complex it is possible to grow your team around it. Indeed it is possible to grow multiple teams around a microservices application. 

Typically with microservices developers are wholly responsible for microservices all the way through to production. **This kind of empowerment is important for startups** because we need all of our developers to be at their most productive. 

# Conclusion

This blog post has outlined the development pipeline and architecture of Sortal as an example of a microservices application. Through this I’ve explained why we chose to use microservices, something of an unorthodox choice that many in the industry would have argued against.

Although microservices-based architectures are great for managing complexity in large complex applications, I have found that they can also be used for smaller applications including MVPs for startups. 

After breaking through the learning curve and looking back I realised that microservices aren’t as difficult as I thought they were, even though it seemed overwhelming at the start of the learning curve. Push through the learning curve, it’s definitely worth it! 

To learn more about building applications with microservices please check out my new book [Bootstrapping Microservices](http://bit.ly/2o0aDsP) and let me help you break through your own learning curve.

In a future blog post we’ll see why and when startups should adopt microservices and look at practical tips on how to get started with microservices on a tiny budget.