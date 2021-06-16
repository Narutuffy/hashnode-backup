## What's wrong with the monolith?


We are hearing about microservices everywhere. This architecture is an increasingly popular alternative to traditional monolithic application development. 

But what is a monolith and what exactly is wrong with it?

In this blog post we'll explore what a monolith is and what is so wrong with it that we’d like to use microservices instead.

This is an extract from chapter 1 of [Bootstrapping Microservices with Docker, Kubernetes and Terraform](http://bit.ly/2o0aDsP).

A monolith is an entire application that runs in a single process. In the era before microservices and although distributed computing has been around for decades, applications were most often built in the monolithic form. This is the way that the majority of software was built before the cloud revolution. Figure 1 shows us what the services in a simple video streaming application might look like and compares the differences for a simple application between a monolithic version and a microservices version.

###### Figure 1. Monolith vs microservices - building with microservices offer many advantages over the traditional monolithic application
![Microservices vs monolith](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828245033/81WqKQeRsc.png)

It is much easier to build a monolith than a microservices application. You need less technical and architectural skills. So it’s a great place to start when starting to build a new application, say in a startup and you want to test the validity of the business model before you commit to the higher technical investment of a microservices application.
A monolith is a great option for early throw-away prototyping. It also might be all that you ever need for an application that has a small scope or an application that stabilizes quickly and does not need to evolve or grow over its lifetime.

Deciding whether to go monolith-first or microservices-first is a balancing act that has traditionally been won by the monolith. However, in my new book [Bootstrapping Microservices](http://bit.ly/2o0aDsP) I’ll show you, given the improvements in modern tooling and with cheap and convenient cloud infrastructure, that it’s important that you consider going microservices-first.

Most products will generally need to grow and to be evolved and it’s actually very difficult to justify throwing away a throw-away prototype. So down the road you might find yourself stuck with the monolith right when you what you really need is the flexibility, security and scalability of a microservices application.

Monoliths come with a host of potential problems. They always starts out small and we always have the best of intentions of keeping the code clean and well organised. A good team of developers can keep a monolith elegant and well organised for many years. But as time passes the vision can be lost or sometimes there wasn’t a strong vision in the first place. All the code runs in the same process, so there are no barriers and nothing to stop us writing spaghetti code. 

Staff turnover has a big effect. As developers leave the team they take crucial knowledge with them and they are replaced by new developers who will have to develop their own mental model of the application and the model they develop might be at odds with the original vision. Time passes, code changes hands many times and these negative forces conspire to devolve the code-base into what is called a big ball of mud. This name denotes the messy state of the application when there is no longer a discernible architecture.

Updating the code for a monolith is a risky affair. It’s all or nothing. When you push a code change that breaks the monolith the entire thing ceases operation, your customers are left high and dry and your company bleeds money. We might only want to change a single line of code, but still we must risk deploying the entire monolith. This risk stokes deployment fear. This fear slows the pace of development.

In addition, as the structure of the monolith degenerates, our risk of breaking it in unanticipated ways increases. Testing becomes harder and breeds yet more deployment fear. Have I convinced you that you should try microservices? Wait there’s more!

**Monoliths come with increased deployment risk:** Pushing a breaking code change, breaks our entire monolith. Increased deployment risk leads to deployment fear. Fear slows the pace of development.

Due to the sheer size of an established monolith it is very problematic to test. Due to its granularity it is very difficult to scale. Eventually the monolith expands to consume the physical limits of the machine it runs on. As the aging monolith consumes more and more physical resources it becomes more expensive to run. I have witnessed this! 
To be fair this kind of thing might be a long way off for any monolith, but even after just a few years of growth the monolith leads to a place that you would prefer not to be.

Despite the eventual difficulties with the monolith, it remains the simplest way to bootstrap a new application. So shouldn’t we always start with a monolith and later restructure when we need to scale? 

My answer: it depends. Many applications will always be small. There are plenty of small monoliths in the wild that do their job well and don’t need to be scaled or evolved. They are not growing and they do not suffer the problems of growth. If you believe your application will remain small, simple and doesn’t need to evolve: you should definitely build it as a monolith.

**Easy to start, but difficult later:** The monolith may be the simplest way to start development, but there are many real and difficult problems we will face further along that road.
However, there are many applications that we can easily predict will benefit from a microservices-first approach. These are the kinds of applications we know will continually be evolved over many years. Other applications that can benefit are those that need to be flexible, scalable or have security constraints from the start. Building these types of applications is much easier if you start with microservices, because converting an existing monolith is difficult and risky.

By all means if you need to validate your business idea first, please do so by first building a monolith. However even in this case I would argue that with the right tooling, prototyping with microservices isn’t much more difficult than prototyping with a monolith. *After all, what is a monolith, if not a single large service?*

You might even consider using the techniques in [Bootstrapping Microservices](http://bit.ly/2o0aDsP) to bootstrap your monolith as single service within a Kubernetes cluster. Now you have the best of both worlds! When the time comes to decompose to microservices, you are already in the best possible position to do so and at your leisure you can start chipping microservices off the monolith. And with the ease of automated deployment that modern tooling offers it is easy to teardown and recreate your application or create replica environments for development and testing. So if you want or need to create a monolith first you can still benefit from the techniques and technologies presented in the book.

Read more about microservices in [Bootstrapping Microservices with Docker, Kubernetes and Terraform](http://bit.ly/2o0aDsP) by Ashley Davis.

![Bootstrapping Microservices](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828245760/jG1RnWrYRY.png)