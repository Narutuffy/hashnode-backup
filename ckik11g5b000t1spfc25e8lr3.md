## The Pursuit of Perfect Pipeline: Part #3 Continuous Integration

We have already talked about [TDD](/posts/pursuit-of-perfect-pipeline-part-1/) and [git branching model](/posts/pursuit-of-perfect-pipeline-part-2/) and now I would like to show you where they meet and why each and every block in this pipeline is so important.

### The word behind the buzz

Continuous integration is a practice where team members integrate their work frequently (even couple of times a day) to the same location, such as a master branch. Each change is going through all tests suites and maybe other verification methods as well to detect errors in recent commit as quickly as possible. If everything goes according the plan, the code is built and sent to an artifact repository, like a docker registry. If the CI practice is applied correctly, it will turn every commit in master branch to a deployable version (a little hint for the next blog post).

### Lots of options in the wild

There are a whole bunch of CI platforms out there, waiting for you to use them. Some are SaaS based, the most famous one of them is Travis CI, and some on-prem like Jenkins. Each platform has its own strengths and weaknesses be sure to understand your team needs before choosing one. It has both financial and ops consequences. We chose Jenkins as our CI platform, for it is the most popular and flexible platform. Of course it comes with a cost, a lot of ops work is needed to maintain the Jenkins cluster and always keep up with the developer needs. We deployed it on top of our Kubernetes cluster for maximum efficiency. Jenkins has a Kubernetes plugin which lets it spawn workers on the fly using containers. This way the Jenkins master is the only instance that is always available and there are no dedicated resources for the workers. [Google Cloud published a detailed post](https://cloud.google.com/solutions/configuring-jenkins-container-engine) about deploying Jenkins to Google Container Engine which is basically a managed Kubernetes cluster, so with a few adjustments you can deploy it to every Kubernetes cluster. In addition, we configured Jenkins to integrate with our GitHub organization, so every repository which contains a `Jenkinsfile` (I’ll elaborate later on), is automatically tested on Jenkins on every commit and PR using webhooks.

![Jenkins](//images.ctfassets.net/de3wzrfouuq1/2EWX0K30NuA8mgkFOHC39i/a07ac6a0ce2bf8350867719a755f7fc9/2.png)

### Building the CI pipeline

Jenkins has this great feature that lets you write a custom pipeline for each project using Groovy in a `Jenkinsfile` (yes, it is the same one from before). This way you can write and manage your CI pipelines the same way you do with your code. In the CI pipeline you should run every test and verification method necessary as long as they are automated and they do not require a lot of compute resources. For example, unit tests, integration tests with local instances if possible (deploy the database on the Jenkins worker using containers or any other way), etc. Load or smoking tests for example should be done in a staging environment not as part of the CI pipeline. Just figure out what are the necessary verification methods for your project and write them in `Jenkinsfile` or the according file for your platform. The big advantage of “Pipeline as Code” is that you are able to write the common functionality as a shared library and reference it from the other projects. This way not every member of the team needs to know how to write a Jenkins pipeline.

Let’s take an example, our own web application pipeline:

*   Check out the code from the git repo.
*   Run Polymer container, a container which contains all the cli tools necessary for building a Polymer project.
*   Install dependencies using yarn and bower.
*   Initialize BrowserStackLocal for forwarding traffic from Jenkins worker to Browser Stack for remote browser testing.
*   Run the test suites on remote browsers using Browser Stack.
*   Build the project.
*   Run a container with gcloud cli tool (we use Google Cloud as you might have already guessed as our cloud platform).
*   Build and push a container with the latest version using Google Container Builder.

Jenkins notifies us on Slack about every job that starts, fails or ends successfully, so most of the time we don’t even bother opening it at all.

![Jenkins Stats](//images.ctfassets.net/de3wzrfouuq1/3NdhSYb4g9IBuK9S8SND2N/0641d1a81b572947627f829504bce8f5/3.png)

Applying CI correctly will improve your code resiliency, help you find bugs and keep your different branches integrated. Do bare in mind that it takes time but as long as you have the will and the spirit you will be just fine.Stay tuned, the next and last blog post is about delivering the product all the way to production - automatically! Not an easy task but totally worth it.
