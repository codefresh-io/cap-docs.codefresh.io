---
title: "Inatall and configure Hosted GitOps"
description: ""
group: runtime
toc: true
---


If you are signed-in to our Hosted GitOps, complete the simple setup, and you are all set to   CD.
Read about Codefresh Hosted GitOps. 

### Where to start with Hosted GitOps
All you need to get started with Hosted GitOps is to install a hosted runtime and complete the configuration for it.  
If you have not installed a hosted runtime, Codefresh presents you with the setup options in the Home dashboard.   


* In the Codefresh UI, go to Codefresh [Home](https://g.codefresh.io/2.0/?time=LAST_7_DAYS){:target="\_blank"}.
  Codefresh guides you through completing the setup, as described below.

{% include 
image.html 
lightbox="true" 
file="/images/runtime/hosted-initial-view.png" 
url="/images/runtime/hosted-initial-view.png" 
alt="Setup steps for hosted runtime in Home dashboard" 
caption="Setup steps for hosted runtime in Home dashboard"
max-width="50%" 
%}

#### 1. Install hosted runtime
Start installing your hosted runtime with a single-click. Codefresh completes the installation without any further intervention on your part. 

{% include 
image.html 
lightbox="true" 
file="/images/runtime/hosted-installing.png" 
url="/images/runtime/hosted-installing.png" 
alt="Step 1: Installation for hosted runtime" 
caption="Step 1: Installation for hosted runtime"
max-width="50%" 
%}

The hosted runtime is installed on the Codefresh cluster, and is completely managed by Codefresh, including version and security upgrades.


1. Click **Install**.
1. When complete, to view the hosted runtime, click **View Runtime**.
  You are directed to the **Runtime Components** tab where you can see all the components in the hosted runtime that you just installed.

{% include 
image.html 
lightbox="true" 
file="/images/runtime/hosted-runtime-components.png" 
url="/images/runtime/hosted-runtime-components.png" 
alt="Hosted runtime: Runtime Components tab after installation" 
caption="Hosted runtime: Runtime Components tab after installation"
max-width="50%" 
%}
  
If you go to the Runtimes page you can see the new hosted runtime in the list.  

 For a hosted runtime, the Cluster/Namespace column displays Codefresh, and the Module column displays CD Ops.

{% include 
image.html 
lightbox="true" 
file="/images/runtime/hosted-runtimes-list-view.png" 
url="/images/runtime/hosted-runtimes-list-view.png" 
alt="Hosted runtime in List view" 
caption="Hosted runtime in List view"
max-width="50%" 
%}



#### 2. Connect Git provider
Connect the managed runtime to a Git provider for Codefresh to create the required repositories. First authorize access to your Git provider through an OAuth token, and then select the Git organization or account in which to create the required Git repos.  

{% include 
image.html 
lightbox="true" 
file="/images/runtime/hosted-connect-git.png" 
url="/images/runtime/hosted-connect-git.png" 
alt="Step 2: Connect Git provider for hosted runtime" 
caption="Step 2: Connect Git provider for hosted runtime"
max-width="50%" 
%}

Once you authorize access, Codefresh creates two Git repositories, one to store the runtime's configuration settings, and the other to store runtime's application settings:
* Shared runtime configuration repo
  The shared runtime configuration repo is a centralized Git repository that stores configuration settings for the hosted runtime. Additional runtimes created for the account can point to this repo to retrieve and use the configuration.  
  Read about [Shared runtime configuration]({{site.baseurl}}/docs/runtime/shared-runtime/).

* Git Source application repo
  Codefresh creates a Git Source application repo for every hosted runtime.  
  Read about [Git sources]({{site.baseurl}}/docs/runtime/git-sources/).


>Hosted runtimes support only OAuth authentication. 


1. Click **Connect**.
1. Select the Git provider, and then click **Authorize Access**, and enter your OAuth token.
  If you don't have an OAuth token, see the instructions on how to generate one in [How to update a Git token]({{site.baseurl}}/docs/administration/user-settings/#how-to-update-a-git-personal-token).  

{% include 
image.html 
lightbox="true" 
file="/images/runtime/hosted-authorize-access.png" 
url="/images/runtime/hosted-authorize-access.png" 
alt="Authorize access to Git provider" 
caption="Authorize access to Git provider"
max-width="50%" 
%}

{:start="3"}
1. Select the **Git Organization for which to create the repos**.    
1. Click **Create**.
  Codefresh creates the two Git repositories in the path shown.
  
{% include 
image.html 
lightbox="true" 
file="/images/runtime/hosted-select-git-repo.png" 
url="/images/runtime/hosted-select-git-repo.png" 
alt="Select Git organization for repos" 
caption="Select Git organization for repos"
max-width="50%" 
%}

{:start="5"}
1. Verify that both repositories have been created in your Git account.
  * Shared runtime configuration repo

{% include 
image.html 
lightbox="true" 
file="/images/runtime/hosted-git-shared-repo.png" 
url="/images/runtime/hosted-git-shared-repo.png" 
alt="Shared runtime configuration repo for hosted runtime" 
caption="Shared runtime configuration repo for hosted runtime"
max-width="50%" 
%}

* Runtime Git Source repo

 {% include 
image.html 
lightbox="true" 
file="/images/runtime/hosted-git-git-source.png" 
url="/images/runtime/hosted-git-git-source.png" 
alt="Git Source repo for hosted runtime" 
caption="Git Source repo for hosted runtime"
max-width="50%" 
%}

{:start="6"}  
1. Optional. To see your tokens, click **View Tokens**. 
1. Return to the Runtimes list, select the hosted runtime you installed, and then select the **Git Sources** tab.  
  You can see the new Git Source created by Codefresh.

 {% include 
image.html 
lightbox="true" 
file="/images/runtime/hosted-git-source-in-ui.png" 
url="/images/runtime/hosted-git-source-in-ui.png" 
alt="Git Source for hosted runtime" 
caption="Git Source for hosted runtime"
max-width="50%" 
%}

#### 3. Connect a Kubernetes cluster
Connect a K8s destination cluster to the hosted runtime and register it as a managed cluster. Deploy applications and configuration to the cluster.  

For managed cluster information, see [Add and manage external clusters]({{site.baseurl}}/docs/runtime/managed-cluster/).

{% include 
image.html 
lightbox="true" 
file="/images/runtime/hosted-connect-cluster-step.png" 
url="/images/runtime/hosted-connect-cluster-step.png" 
alt="Step 3: Connect a K8s cluster for hosted runtime" 
caption="Step 3: Connect a K8s cluster for hosted runtime"
max-width="50%" 
%}


1. Click **Connect**.
1. In the Add Managed Cluster panel, do the following:
  * **Cluster Context**: Enter the context name for your cluster, as it appears in your kubeconfig file. 
  * Define the parameters and then run the command:  
    `cf cluster add <runtime-name> --context <context_name> [--dry-run]`  
    where:  
      `<runtime-name>` is the runtime to which to register the cluster. The name of the selected runtime is automatically added.  
      `<context_name>` is the kube context with the credentials to communicate with the managed cluster. If not supplied, the CLI displays the list of available clusters as defined in `kubeconfig`.  
      `--dry-run` is optional, and required if you want to generate a list of YAML manifests that you can redirect and apply manually with `kubectl`.
  
{% include 
image.html 
lightbox="true" 
file="/images/runtime/managed-cluster-add-panel.png" 
url="/images/runtime/managed-cluster-add-panel.png" 
alt="Add Managed Cluster panel" 
caption="Add Managed Cluster panel"
max-width="50%" 
%}

{:start="3"}
If you 
You have completed setting up your hosted runtime, and can now deploy applications to the managed cluster, connect third-party CIs to enrich image information.



#### (Optional) Create application
Create an application in Codefresh, deploy it to the cluster, and track deployment and performance in the Applications dashboard.  

[Create an application]({{site.baseurl}}/docs/deployment/create-application/)  
[Applications dashboard]({{site.baseurl}}/docs/deployment/applications-dashboard/)

#### (Optional) Connect CI 
Integrate third-party tools you use for CI with Codefresh to enrich the image information in deployments.







