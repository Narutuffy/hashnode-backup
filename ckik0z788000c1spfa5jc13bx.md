## Looking Through The Developer’s Eyes For Product Managers — Part 2

[Previously](/posts/looking-through-the-developers-eyes-for-product-managers-1/) I introduced you to some of the keys for a successful applications development which every product manager must know. In this post, I want to dive deeper into the technical terms and concepts in development, so you can understand the developers’ language.

#### Cleaning Up The Mess

**Collaborating with different team members can become a real mess if not done correctly.** Lucky for us, developers, there are many version control platforms to help us collaborate easily without an effort.

The version control system tracks the code changes of each one of the team members. This way when different team members work on the same file, they can review the changes and merge the them correctly.

Without any doubt, the most popular platform is [Git](https://git-scm.com/), which was initially designed for the Linux kernel development.

The following list of Git terms will make the developers casual talk a bit clearer to you:

*   Commit — saves the current changes locally. When committing, one must provide a message with detailed explanation about the changes.
*   Push — sends the local changes into a central Git server so others can pull them.
*   Pull — updates the local code with other team members changes using the central Git server.
*   Merge — when two members edit the same line, one of them must decide how to apply these changes into the file. When possible Git will merge the changes automatically.
*   Tag — marks a specific commit in order to easily revert your code back to this state. Usually used for releasing new versions.

There are two well known Git hosting services:

*   [Github](https://github.com/) — The most common git server, almost all open source projects are hosted there and it can be integrated with a lot of online services.
*   [Bitbucket](https://bitbucket.org/) — Used mostly for hosting private projects because of its free plan.

#### Reveal Your Magic

Nowadays, everyone realizes the importance of sharing and opening their source code. Just recently, Google has open sourced _Tensor Flow_, its machine learning library. Twitter has _Bootstrap_, its front-end framework. Facebook shared _React &amp; React native_, its library for building user interfaces and a lot more.

By opening the source code you simply crowd source the development and let other developers around the world to contribute to your project. In addition, you get the developers sympathy for making their life easier, thus getting publicity and reputation for the company. The companies even “fight” for this kind of sympathy because of its great power.

**As a manager, encourage your developers to contribute back to open source projects and even publish core libraries and frameworks of the company.** Although it adds overhead time to manage the project and takes more effort to maintain it, opening the source can lead the project to places you didn’t even imagine.

As a developer, I would rather use open source library rather than programming my own. **But it is very important to check the development activity and reputation of this specific library before using it.** One can get this information by checking the last commit date, how many people starred or forked it, etc.

#### Clouds Are Everywhere

I am sure you have already heard about cloud computing thousand of times but you are probably not so sure what it means. The cloud consists of two major components. First, high computing power, usually scattered in different data centers around the world. Second, web services to ask for these resources on demand. This way your application can ask for low computing power when the users are asleep and for high computing power in rush hour. If you do the math, **you will realize that it can save your company a lot money for maintaining unnecessary servers.**

There are three types of cloud services:

*   IaaS (infrastructure as a service) — The most basic service a cloud can provide. You get a virtual machine with an operating system of your choice (Ubuntu, of course) to configure, install, deploy and maintain whatever you want, just like your PC at home.
*   PaaS (platform as a service) — Simply deploy your app to a GIT repository or with a command line tool of the cloud vendor and the cloud will take care of the rest. The cloud will provision and maintain the relevant computing resources to run your app.
*   SaaS (software as a service) — This is the most abstract service a cloud can provide, the software itself as a service. Gmail, Wix and Facebook are just a few SaaS examples that you are familiar with.

Each type has its pros and cons but they are out of this post scope.

There are a lot of cloud providers, each one of them has its own services, technologies, pricing model and performance, **so choose wisely according your needs.** Here is a list of a few cloud providers:

*   [Amazon Web Services](https://aws.amazon.com/)
*   [Google Cloud Platform](https://cloud.google.com)
*   [Heroku](https://www.heroku.com/) (PaaS only)
*   [Microsoft Azure](https://azure.microsoft.com) (one of the fewest which supports .Net as PaaS)

#### The Rise Of The Containers

Container is a tool which enables encapsulation of the deployment environment into a single file, much like virtual machine. The main difference here is that container uses the kernel of the host OS, whereas a VM contains its own installation of OS. **This enables container to startup in a matter of seconds or even milliseconds and doesn’t require overhead memory or CPU.** But this means you can’t run Linux containers on Windows host or vice-versa (bare in mind that containers in Windows are still experimental). Because of container amazing adoption, cloud providers integrates it as a core functionality and part of their PaaS.

Docker, an abstraction of Linux containers, makes using containers a whole lot easier. You can create a docker using a dockerfile which is simply a text file. In the dockerfile one should define the base image (for example a bare ubuntu installation, a nodejs envrionment, etc), the instructions to bootstrap the container (copy files, run bash commands, etc) and finally the entry point of the container, which is basically the main process of the container. Using the docker command line tool one can build the container, upload it to a docker registry and then run it everywhere. **Conceptually a container should focus on one well defined task and do it right (running a MySQL database, a Scala web server, etc).**

In addition to the command line tools, there is the [Docker Hub](https://hub.docker.com/) which is a marketplace for dockers. You can find a docker for almost anything you want in many variations and in different versions, some of them are even open source. The Docker ecosystem helps developers to deliver quicker and effortless.

#### Test It Till You Make It

Automated testing is probably the most neglected matter when it comes to managers and even for some developers and it shouldn’t be!

When a car is shipped out from the factory, one expects it to be well tested and work as it should. This could not have been done without automatic validation of each and every car that leaves the factory. It could have been done with manual testing but it could not keep up the pace of the demand. So why do you treat your application differently? **Your users expect to get high-end product which keeps updating and getting better each day.** Just like in the car factory, you must have an automatic testing and validation pipeline to ensure you deliver the greatest product. But it is not all sunshine and roses there are downsides too. Writing and maintaining these tests take a lot of time. Managers measure progress by counting features which is wrong **because without tests your product is vulnerable and can break anytime.** The developers at Amazon deploy new version every day and even more thanks to automatic testing.

Next time when you sit to plan your sprint take into account that your developers need time to write tests and encourage them to do so!

#### Conclusion

In this series of posts, I wanted to narrow the gap between product managers and developers. These posts are just a taste to the enormous world of development. Keep reading, learning and implementing and you will build amazing products.
