---
title: "Get started with Hosted GitOps"
description: ""
group: runtime
toc: true
---


If you are signed-in to our Hosted GitOps and have , you only need to complete the simple setup t, and you are all set to leverage our extensive CD Ops capabilities.
Read about Codefresh Hosted GitOps. 

### Where to start with Hosted GitOps
* In the Codefresh UI, go to Codefresh [Home](https://g.codefresh.io/2.0/?time=LAST_7_DAYS){:target="\_blank"}.
  Codefresh guides you through the three-step setup, as described below.

{% include
image.html
lightbox="true"
file="/images/runtime/hosted-initial-view.png"
url="/images/runtime/hosted-initial-view.png"
alt="Provision hosted runtime"
caption="Provision hosted runtime"
max-width="70%"
%}

#### 1. Install hosted runtime
Start installing your hosted runtime with a single-click. Codefresh completes the installation without any further intervention on your part. 
The hosted runtime is installed on the Codefresh cluster, and completely managed by Codefresh with automatic version and security upgrades.

1. Click **Install**.

{% include
image.html
lightbox="true"
file="/images/runtime/hosted-installing.png"
url="/images/runtime/hosted-installing.png"
alt="Installing hosted runtime"
caption="Installing hosted runtime"
max-width="70%"
%}

{:start="2"}
1. When complete, to view the components for the hosted runtime, click **View Runtime**.
  You are taken to the Runtime Components tab.  

{% include
image.html
lightbox="true"
file="/images/runtime/hosted-initial-view.png"
url="/images/runtime/hosted-initial-view.png"
alt="Runtime components for hosted runtime"
caption="Runtime components for hosted runtime"
max-width="70%"
%}

> The Git Sources and the Managed Clusters are empty as they will be set up in the next steps.  

If you navigate to **Runtimes > List View**, the Cluster/Namespace column displays Codefresh, and the Module column displays CD Ops indicating a hosted runtime.

{% include
image.html
lightbox="true"
file="/images/runtime/hosted-runtimes-list-view.png"
url="/images/runtime/hosted-runtimes-list-view.png"
alt="Hosted runtimes in List view"
caption="Hosted runtimes in List view"
max-width="70%"
%}



#### 2. Connect Git provider
Connect your hosted runtime to a Git provider. Codefresh creates the required runtime Git repos for you.  First authorize access to your Git provider through an OAuth token, and then select the Git organization or account in which to create the required Git repos.  

{% include
image.html
lightbox="true"
file="/images/runtime/hosted-connect-git.png"
url="/images/runtime/hosted-connect-git.png"
alt="Connect to Git provider"
caption="Connect to Git provider"
max-width="70%"
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
1. Click **Authorize Access**, and enter your OAuth token.
  If you don't have an OAuth token, see the instructions on how to generate one in [How to update a Git token]({{site.baseurl}}/docs/administration/user-settings/#how-to-update-a-git-personal-token). 

{% include
image.html
lightbox="true"
file="/images/runtime/hosted-authorize-access.png"
url="/images/runtime/hosted-authorize-access.png"
alt="Authorize access to Git"
caption="Authorize access to Git"
max-width="70%"
%}

{:start="3"}
1. Select the **Git Organization for which to create the repos**.
1. Click **Create**.
  Codefresh creates the two Git repositories in the paths shown.

 {% include
image.html
lightbox="true"
file="/images/runtime/hosted-authorize-access.png"
url="/images/runtime/hosted-authorize-access.png"
alt="Git configuration repos for Git Organization"
caption="Git configuration repos for Git Organization"
max-width="70%"
%}

{:start="5"}
1. Verify that both repositories have been created in your Git account.
  Shared runtime configuration repo
  
  {% include
image.html
lightbox="true"
file="/images/runtime/hosted-git-shared-repo.png"
url="/images/runtime/hosted-git-shared-repo.png"
alt="Shared configuration repo in Git"
caption="Shared configuration repo in Git"
max-width="70%"
%}

  Runtime Git Source repo

{% include
image.html
lightbox="true"
file="/images/runtime/hosted-git-shared-repo.png"
url="/images/runtime/hosted-git-shared-repo.png"
alt="Shared configuration repo in Git"
caption="Shared configuration repo in Git"
max-width="70%"
%}

{:start="6"}  
1. Optional. To see your tokens, click **View Tokens**. 

If you return to the Runtimes page and select the Git Source tab, you will now see the Git Source that Codefresh created.  
The Sync State is Unknown as it is still not synced to a cluster. 

{% include
image.html
lightbox="true"
file="/images/runtime/hosted-git-source-in-ui.png"
url="/images/runtime/hosted-git-source-in-ui.png"
alt="Git Source tab for hosted runtime in UI"
caption="Git Source tab for hosted runtime in UI"
max-width="70%"
%}


#### 3. Connect a Kubernetes cluster
Connect a destination cluster to the hosted runtime and register it as a managed cluster. Deploy applications and configuration to the cluster.
For managed cluster information, see [Add and manage external clusters]({{site.baseurl}}/docs/runtime/managed-cluster/).
SCREENSHOT


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

You have completed setting up your hosted runtime, and can now deploy applications, connect third-party CIs.

#### (Optional) Create application
Create an application in Codefresh, deploy it to the cluster, and track deployment and performance in the Applications dashboard.  

[Create an application]({{site.baseurl}}/docs/deployment/create-application/)  
[Applications dashboard]({{site.baseurl}}/docs/deployment/applications-dashboard/)

#### (Optional) Connect CI 
Any third-party tools for CI can be integrated with Codefresh to enrich the image information for ????.






