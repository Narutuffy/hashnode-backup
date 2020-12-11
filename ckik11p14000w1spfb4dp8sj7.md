## Kubernetes: The Good, the Bad and the Ugly

[Kubernetes](https://kubernetes.io/) is probably one of my favorite DevOps platforms, and too often I get asked by other developers why.
I think that today, many developers tend towards using fully-managed application platforms such as [Now](https://zeit.co/), [App Engine](https://cloud.google.com/appengine), [Heroku](https://www.heroku.com/) and many others. This makes sense as I use them too but in a system's life there comes a time that it's simply not enough.

I would like to guide you into the past and tell the story of [Daily](https://daily.dev/), my side project which turned out to be very popular among developers.
When we just started Daily all we needed is a single page application that should be packed as a browser (Chrome/Firefox) extension and a monolith API application that delivers the best dev news to the extension. Our hosting needs were pretty straight-forward so as a Google Cloud fanboy, I decided to deploy the API server to App Engine. You deploy code and behind the scenes App Engine provisions servers, SSL certificates and drive traffic.
The project grew and with it, we had to develop more components to the system, user authentication service, background processes, and cronjobs to name a few. We could go on and search a different tool for every component but I was already familiar with Kubernetes so I knew it's exactly what I'm looking for. It took a while but I migrated everything to Kubernetes and now I have one platform. The platform can handle almost every DevOps need that I have, from the network level to application level. Kubernetes is not all good, there is a steep learning curve and operations overhead that you have to take into account but it's worth it.

# What is Kubernetes?

Kubernetes (K8s) is an open-source system for automating deployment, scaling, and management of containerized applications. You state how many instances of a given container you would like to deploy and Kubernetes will do it for you.

# Docker as a First-Class Citizen

Many platforms have limited runtime support, for example, some support only Node, and others many run only Java and sometimes even only a specific version. As developers, this can limit our choices and our desire to be on top of the latest tech. With [Docker](https://www.docker.com/) you can simply package any application you want and deploy it. Docker has a great community and eco-system so you can even utilize existing images, not just your own. Kubernetes is a docker orchestration platform, so it's all about Docker. If you have a Docker image of your app, Kubernetes will take care of it for you. You can use the latest Node version, Golang, or even a language that only you know.

# Infrastructure as Code

Everything and I really mean everything in Kubernetes is YAML based. Meaning that when you want to deploy your app you write YAML, when you want to increase the scale you again write YAML and these YAMLs can be committed to your Git repo. I can't stress out how important it is to have your infrastructure as code and versioned in Git. You can easily create an infinite number of environments to run, test and verify your app. You can also automate the process of deploying a new version of the app, now that everything is code-based.

# One Stop Shop

Kubernetes can fulfill most of your requirements, as it's packed with these awesome features out-of-the-box:
* Service discovery
* Load balancing
* Autoscaling
* Self-healing
* Secret and configuration management
* Private networking
* Cronjobs

You can extend it even further to automatic provisioning of TLS certificates, database management and more by installing open-source tools in your cluster.

You can choose a deployment strategy that will fit your app needs, canary, blue/green, ramped and more.

Very little is not (or can be) covered by Kubernetes and requires an outside solution.

# Fully-Managed Service

I use Google Kubernetes Engine (have I mentioned that I'm a Google Cloud fanboy?) for my clusters. It's a Kubernetes as a service provided by Google. The awesome part is that Google charges nothing for the service, you pay only for the instances you run. This gives no reason to provision it by yourself. Google takes care of patching your cluster, keeping it alive and that everything is running. A dream comes true!

# The Bad

I will not lie Kubernetes comes with a burden. It is mainly involving learning the platform  First of all, it can be intimidating to deep dive and learn it. Kubernetes has a pretty steep learning curve and takes time to learn and master. It requires DevOps skills and infrastructure knowledge. Monitoring, logging and alerting are a must with Kubernetes, many platforms will help you nail it but it still takes time.


Kubernetes is far from being perfect but it's definitely going there. If you can wield its power and master it can become your platform of choice. You just have to make sure the timing is right and be ready to learn. The journey is not easy but worth every second! 
