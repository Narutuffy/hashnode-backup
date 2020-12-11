## Dockerizing Your Scala Application

A docker image is one of the best and simple ways to ship applications today. The docker image contains everything the application needs to run, so the host machine should have only a docker client installed. Furthermore, it is very easy to manage this kind of artifacts as docker registries are all over the place and available for public use.

Although I found out building a docker image for scala app is pretty easy, it is not well documented in the internet. So I would like to shed some light and aid you with your next scala project.1. We need to build an independent jar file which contains the application as well as its dependencies. Lucky for us, this is called “fat jar” and we can easily build it with [sbt-assembly](https://github.com/sbt/sbt-assembly). Add the following line to your `plugins.sbt` :
`addSbtPlugin("com.eed3si9n" % "sbt-assembly" % "0.14.5")`

As of writing this post version `0.14.5` is the newest. We have to tweak `build.sbt` a little bit, as well, to add assembly configuration:

```
 // No need to run tests while building jar
 test in assembly := {}
 // Simple and constant jar name
 assemblyJarName in assembly := s"app-assembly.jar"
 // Merge strategy for assembling conflicts
 assemblyMergeStrategy in assembly := {
   case PathList("reference.conf") => MergeStrategy.concat
   case PathList("META-INF", "MANIFEST.MF") => MergeStrategy.discard
   case _ => MergeStrategy.first
 }
```

Now we can easily build a fat jar using `sbt assembly` command that will build it to `target/scala-2.xx/app-assembly.jar` .2. We need to find a base image for our docker image. A rule that I am always trying to keep when building a docker image, is to make it as lightweight as I can without adding anything which is not necessary. Scala is a JVM language so I figured that jre might be the only thing I need to run it, no need for sbt or jdk, these packages are big and needed for development only. Going through the Docker Hub reveals `openjdk` image with `8-jre-alpine` tag, which means that the image is based on alpine linux and contains jre 8. Alpine linux is always my way to go as it contains almost nothing. The base image size is ~80MB and my far jar size ~50MB, so expected container size ~130MB, which is pretty lean!3. Our last station, time to build our docker image. The Dockerfile itself is pretty straightforward:
```
FROM openjdk:8-jre-alpine
RUN mkdir -p /opt/app
WORKDIR /opt/app
COPY ./run_jar.sh ./app-assembly.jar ./
ENTRYPOINT ["./run_jar.sh"]
```

We have to copy our fat jar and a bash script (I’ll explain it later on) to the image and set the bash script as an entry point. One important note, please set the path of these two files according to your directory structure.

The bash script allows us the provide runtime arguments for the application or the JVM. It simply switches the order of the arguments and jar name when running `java` :
`#!/usr/bin/env sh``java $* -jar app-assembly.jar`

Now to build the image itself first build the fat jar using `sbt assembly` then run `docker build -t scala-app ./` , you can choose whatever tag name you want instead of `scal-app` and make sure to point docker build to the directory of the Dockerfile.That’s it we are done! We now have a docker image that we can deploy to wherever we want (VMs, Kubernetes, Docker Swarm, etc). I also created a gist as medium is not the best place to share code. [https://gist.github.com/idoshamun/370f19c45832281b68b4e3cfc6f8536d](https://gist.github.com/idoshamun/370f19c45832281b68b4e3cfc6f8536d)

