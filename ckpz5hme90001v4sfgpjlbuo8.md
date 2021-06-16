## The quick guide to creating a Kubernetes cluster


Did you ever find yourself needing a Kubernetes cluster quickly because you wanted to test an idea or run an experiment?

Being able to create a Kubernetes cluster quickly is really useful whether you are writing books and blog posts on it (like me) or even if you are just wanting to learn Kubernetes and need an empty cluster to play with.

If that sounds appealing, please read this post and even follow along with it to build your own cluster. This is intended to be the simplest and quickest way to produce a Kubernetes cluster in the shortest amount of time. I've created Kubernetes clusters so many times I thought it was time to write it down - if only for my future self for the next time I need to do this.

![Kubernetes!](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828158699/jySXj56gH.png)

## Why Azure?

I create my [Kubernetes clusters on Azure](https://azure.microsoft.com/en-au/services/kubernetes-service/). Why is that? It's because it's easy and to a large extent it's what I know. I also enjoy the experience and the Azure cli tool makes a lot of sense. Microsoft have gone out of their way to make Azure friendly to developers and it shows, recently I tried to do this on AWS and it was way more complicated.

Azure have $200 credit for new sign ups, so you can play with this stuff for a month before you start paying real money for it.
Just make sure you tear it all down afterward, otherwise you'll have to start paying for it!

## Azure cli

In this post I use the Azure cli tool from the terminal to construct a Kubernetes cluster. It's a nice tool, pretty straight forward and you can do everything without leaving the terminal. It is even easier to do this kind of thing using the UI in the [Azure portal](https://portal.azure.com/), but that's not automatable. 

The nice thing about working with the terminal is that you can build shell scripts to do this kind of thing. So once you have read this post and understood the commands you can put them all in a "make_kubernetes_cluster.sh" shell script and use it to quickly instantiate a new cluster anytime you need one.

In my new book [Bootstrapping Microservices](http://bit.ly/2o0aDsP) I show how to create a Kubernetes cluster using a Terraform script.

## Pre requisites

You need an Azure account. Login or sign up at https://portal.azure.com/.

You need the Azure cli tool installed: https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest


## Setup

First open a terminal and check that the Azure cli tool is installed:

```
az --version
```

You should see output similar to this:

    azure-cli (2.0.28)

To login to your Azure account from the terminal invoke this command:

    az login

Now follow the instructions in your terminal to authenticate.

After doing this you can run Azure cli commands to create, destroy and update resources in your Azure account.

## Check your account

Before proceeding let's check your account.

Invoke this command:

    az account list

By itself that command outputs in JSON format. If you only just signed up to Azure or if you only have one account, you should see only a single account in that list.

If you do happen to have a lot of accounts in the list, you can make the output more readable by using the table output format as follows:

    az account list --output table

Note which of your accounts has `IsDefault` set to `True`. If you only have one account it should be that one. The default account is the one that we'll be working with. When you create your Kubernetes cluster and other resources, they will appear in that account. 

So we need to make sure the account we want to work with is the default one. If you have work accounts in your list you probably don't want to be making experimental Kubernetes clusters in them!

Here's another way to see just what your default Azure account is:

    az account show

If that isn't the account you want to use for this experimental work, you'll need to change the default account. Use `az account list --output table` and pick the account you want to use. Take note of the `SubscriptionId` for that account. You need to provide this number to set that account as the default.

Use the following command to set your default account:

    az account set --subscription <your-subscription-id>

Please replace &lt;your-subscription-id&gt; with the particular subscription ID for your account.

After changing the default account, check one last time to make sure you are using the account you think you are:

    az account show

## What location?

Before you can decide which version of Kubernetes to install you need to know the location of the data center where you'll create your cluster.

Get a list of all location using this command:

    az account list-locations

Actually that list is pretty long. Let's use [JMESPath](http://jmespath.org/) to pluck a subset of the data to make it easier to read:

    az account list-locations --query "[][name, displayName]"

Still difficult to read though, so now let's use the tsv output format to improve that:

    az account list-locations --query "[][name, displayName]" --output tsv

Now you should have a readable list of locations. Pick a location and take note of the lower case whole word version of its name.

For this blog post I've picked `australiaeast` as the location for my cluster.

## What version?

With your location selected you can now check out which version of Kubernetes are available that location:

    az aks get-versions --location australiaeast

Just replace `australiaeast` with your  prefered location.

Again we are presented with a wall of JSON data. We'll use JMESPath to pluck the most recent version with tsv output for readbility:

```
az aks get-versions --location australiaeast --query "orchestrators[-1].orchestratorVersion" --output tsv
```

Take note of the version number. At the time of writing, the latest version in my location is `1.14.8` so that's the version I'm using for this blog post.

## Create a resource group

We need to create an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) to contain your cluster:

    az group create --name my-kub-test --location australiaeast

A resource group is a way to collect and manage groups of cloud resources. 

I've called my resource group `my-kub-test`, you should probably choose a better name. Prefer a name that indicates your intended use of the resource group.

You can test that your resource group was created with this command:

    az group list --output table

You should see your new resource group in the output. If it's the first group you created, that's all you'll see.

## Create your Kubernetes cluster

We are now ready to create our Kubernetes cluster. Here's the command:

    az aks create --resource-group my-kub-test --name test-kub-cluster --location australiaeast --kubernetes-version 1.14.8

Now you have to wait! This part can take some time, so go make a coffee.

## Check your cluster

Let's check your cluster is ok. List your resource groups:

    az group list --output table

You should have a couple of extra resource groups now. Of course there's `my-kub-test` that we explictly created earlier and contains your managed Kubernetes cluster.

You'll note there's a couple of other resource groups that have been automatically created. The first has a long generated name like `MC_my-kub-test_test-kub-cluster_australiaeast`. That name might be different for you because it's generated from the name of your resource group, the name of your cluster and your selected location. This group contains additional resources created for the managed cluster. We'll have a look at it's  resources in a moment.

There's another group [NetworkWatcherRG](https://azure.microsoft.com/en-us/updates/azure-network-watcher-will-be-enabled-by-default-for-subscriptions-containing-virtual-networks/) that is automatically created. This has something to do with debugging and trouble shooting for your virtual network (which was created automatically for you).

Now try listing the resources in your `my-kub-test` resource group:

    az resource list --resource-group my-kub-test --output table

You should see just one resource (unless you started with a resource group that already had existing resources in it).  The resource you should see is your managed Kubernetes cluster.

Now take a look at the contents of the automatically generated `MC_my-kub-test_test-kub-cluster_australiaeast` resource group

    az resource list --resource-group MC_my-kub-test_test-kub-cluster_australiaeast --output table

This time you should see a bigger list of resources. These are all the resources that were automatically created to support your managed Kubernetes cluster. In the list you'll see things like virtual machines, virtual disks and virtual networks. This is all the stuff you are paying for! (after your $200 credit runs out). Be sure to delete it when you are done! Also try not to manually tweak anything in this list! If you do that could break your 
cluster.

## Interface with your cluster 

What good is a Kubernetes cluster if you can't interact with it to deploy your application? So let's get setup for that.

For this you need the Kubectl tool installed: https://kubernetes.io/docs/tasks/tools/install-kubectl/

After installing Kubectl, use the following command to download credentials for your cluster:

    az aks get-credentials --resource-group my-kub-test --name test-kub-cluster

This downloads credentials so you can run Kubectl and interact with your cluster.

Test run with the following command:

    kubectl get nodes

You should see a list of the nodes in your cluster! You can now use Kubectl manage your cluster. Happy days.

## Tear down

When you are done don't forget to tear down your cluster and all the resources that were created.

Azure resource groups make this easy, just delete the main resource group `my-kub-test`:

    az group delete --name my-kub-test

When you delete the resource group with your cluster the other one that was automatically created is also deleted. That's nice.

Unfortunately the group `NetworkWatcherRG` doesn't get cleaned up, but you can delete that explictly if you like:

    az group delete --name NetworkWatcherRG


## Conclusion

If you followed along with this post you created (and then destroyed) a Kubernetes cluster. This is quick and simple way to create a managed Kubernetes cluster for those times when you want one to experiment on and try out new stuff. Have fun with your Kubernetes cluster!

## Resources:

- Kubernetes on Azure: https://azure.microsoft.com/en-au/services/kubernetes-service/
- Installing kubectl: https://kubernetes.io/docs/tasks/tools/install-kubectl/
- Kubectl cheat sheet: https://linuxacademy.com/site-content/uploads/2019/04/Kubernetes-Cheat-Sheet_07182019.pdf
- Azure cli ref: https://docs.microsoft.com/en-us/cli/azure/reference-index?view=azure-cli-latest
- Azure workshop: https://aksworkshop.io/
- JMESPath: http://jmespath.org/ 
- Azure cli query: https://docs.microsoft.com/en-us/cli/azure/query-azure-cli?WT.mc_id=ITOpsTalk-blog-nepeters&view=azure-cli-latest
- Azure cli output formats: https://docs.microsoft.com/en-us/cli/azure/format-output-azure-cli?view=azure-cli-latest