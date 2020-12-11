## The Pursuit of Perfect Pipeline: Part #2 Git Branching Model

[After we understood the importance of TDD and automatic testing](/posts/pursuit-of-perfect-pipeline-part-1/), it is time to choose the best fit git branching model for the product. The branching model is one the most fundamental building blocks of the software pipeline.

The branching model describes how as a developer you interact with the source control platform (usually git), how to work with the different branches, when to checkout a new branch and even from which branch you can checkout a new branch.

Choosing the right branching model is quite easy, if you know the existing options and the needs of your project. It is of course an evolving process with some adjustments along the way.

### The importance of branching model

I have seen teams which work without a pre-defined branching model, everyone decide differently when to checkout a new branch, when to merge back and to which branch to merge. A total chaos, you neither can keep track of what is happening in your code, nor be sure that you are always up-to-date.

With the right branching model, you can “force” your development methodology. For example, if restricting push to master branch and allowing only pull requests to be merged, you can “force” code reviews , thus improving the code quality. You can decide which branches should be deployed to which environment or what tests should run on which branches. The branching model can be an enabler for a lot of processes which make your code more resilient, higher quality and at the end a better product.

### There are only two options on the table

As I see it, there are only two relevant models for most projects. The first one, my default, is [GitHub Flow](https://guides.github.com/introduction/flow/). There is a beautiful designed and well-written explanation [in the link](https://guides.github.com/introduction/flow/). So I will summarize the main idea. The key idea here is to make things simple and keep integrating with each other. So branches are checked out from **master only**. The branch should cover a specific task or a user story and not a long lasting process, usually should be merged back to master in a matter of a few days even less. When done, you should create a pull request and it is a good time for a code review and to run tests. Notice that master branch always contains a deployable version of the project, we will take advantage of this concept in the next posts in the series. Because of its simplicity I like to use GitHub Flow for most of my projects.

The other option is [GitLab Flow with release branches](https://docs.gitlab.com/ce/workflow/gitlab_flow.html#release-branches-with-gitlab-flow). Use this model for platforms where the deployment can not be fully automated and requires also a pro-active action of the user. For example, releasing an Android application to Google Play requires a confirmation of Google and an update by the user. The same is true for iOS, hardware projects, etc. Due to these manual steps, it is harder to rollback and release bug fixes found in production. As a result, the model for this kind of projects must keep track of the releases and should enable simple bug fixing for a specific release while not affecting the feature development. Release branches with GitLab Flow aims to solve exactly this matter. The same as GitHub Flow branches are being merged only to master, but this time we checkout a new branch for each minor release and cherry-pick only the relevant commits from the master to the release branch. For example, let’s say we just released version 1.1.0 to Google Play from branch “1.1-stable” and then found a bug. We fix the bug in a new branch we checked out from master, fix it and merge back to master. Then we cherry-pick this commit to our release branch “1.1-stable”, build and deploy.

### Start today!

There is no excuse to start using a branching model, it does not require a lot of effort, maybe a bit of guidance to the other team members but it is worth your time. Choose the right fit for you and implement it even today in your project. To continue in the process of building the perfect pipeline you must have a branching model, otherwise you couldn’t achieve the full potential of the pipeline. [Coming up next](/posts/pursuit-of-perfect-pipeline-part-3/), a chapter about continuous integration which fits perfectly with the two branching models above. Continuous integration will help you gain trust in your code and better resiliency.
