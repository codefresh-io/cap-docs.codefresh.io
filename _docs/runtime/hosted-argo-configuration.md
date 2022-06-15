---
title: "Inatall and configure Hosted GitOps"
description: ""
group: runtime
toc: true
---


If you have signed-in for our Hosted GitOps offering, complete the three-step setup, and you are all set to launch your CD.

### Set up hosted runtime in Codefresh 
In the Codefresh UI, go to te 

#### 1. Install hosted runtime
Start the installation. Codefresh completes the installation without any further intervention on your part. 

    SCREENSHOT

   1. Click **Install**.
   1. To view the hosted runtime, click **View Runtime**.
      You are taken to the Runtimes page with the list of runtimes.  
   
      For hosted runtimes, the Cluster/Namespace column displays Codefresh, and the Module column displays CD Ops.
      SCREENSHOT



#### 2. Connect Git provider
Connect the managed runtime to a Git provider. First authorize access to your Git provider through an OAuth token, and then select the Git organization or account in which to create the Git repos.  
SCREENSHOT


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
1. Select the **Git Organization for which to create the repos**.
1. Click **Create**.
  Codefresh creates the two Git repositories in the path shown.
  SCREENSHOT
1. Verify that both repositories have been created in your Git account.
  Shared runtime configuration repo
       SCREENSHOT
  Runtime Git Source repo
   SCREENSHOT

{:start="6"}  
1. Optional. To see your tokens, click **View Integrations**. ???


#### 3. Connect a Kubernetes cluster
Connect a destination cluster to the hosted runtime, and register it as a managed cluster. Your applications and configuration are deployed to the cluster.
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

You have completed setting up your hosted runtime, and can start deploying applications or connecting third-party CIs.

#### (Optional) Create application
Create an application in Codefresh, deploy it to the cluster, and track deployment and performance in the Applications dashboard.  

[Create an application]({{site.baseurl}}/docs/deployment/create-application/)  
[Applications dashboard]({{site.baseurl}}/docs/deployment/applications-dashboard/)

#### Connect CI 
Any third-party tools for CI can be intergrated with Codefresh to enrich the image information for ????.






