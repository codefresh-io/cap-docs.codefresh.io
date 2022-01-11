---
title: "WIP: 3. Install runtime"
description: ""
group: getting-started
toc: true
---


Installing the runtime installs the Codefresh Software Development Platform (CSDP), comprising Argo project components and CSDP-specific components. We maintain an enteprise-supported version of the Argo CD components, derived from a conformed fork of the Argo ecosystem.

### About runtime installation
There are two parts to installing runtimes:  
1. Installing the CSDP CLI, a one-time action, typically required only for initial CDSP setup.  
2. Installing the CSDP runtime from the CLI. The runtime is installed in a specific namespace on your cluster. You can install more runtimes on different clusters in your deployment.  
  Every runtime installation makes commits to two Git repos: 
   * Runtime install repo: The installation repo that manages the runtime itself with Argo CD. If the repo URL you provide does not exist, CSDP untime creates it automatically.   
   * Git Source repo: Created automatically during runtime installation. The repo with the demo resources required for the sample `Hello World` pipelines we provide. 

### Before you begin
A runtime requires a Git token for authentication to the Git installation repository.
Have your GitHub Personal Authentication Token (PAT) ready with a valid expiration date and access permissions:
* Expiration: Either the default of 30 days or any duration you consider logical.
* Access scope: Set to `repo`

  {% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/github-pat.png" 
   url="/images/getting-started/github-pat.png" 
   alt="GitHub PAT permissions" 
   caption="GitHub PAT permissions"
   max-width="30%" 
   %}  

  If you need detailed information on GitHub tokens, see the [GitHub article](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

### Download the CSDP CLI
Downloading the CSDP CLI requires you to select the download mode and OS, generate an API key and authentication context, as instructed in the CSDP UI.
1. In the Welcome page, select **+ Install Runtime**.
1. Download the Codefresh CLI:
  * Select one of the methods. 
  * Generate the API key and create the authentication context. 
    {% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/quick-start/quick-start-download-cli.png" 
   url="/images/getting-started/quick-start/quick-start-download-cli.png" 
   alt="Example Git Hub repo with demo resources" 
   caption="Example Git Hub repo with demo resources"
   max-width="30%" 
   %} 
### Install CSDP runtime
For the quick start, you will install the runtime through the CSDP CLI that you downloaded previously. 

1. To start runtime installation, run `cf runtime install`.
1. Follow the prompts in the CLI wizard to complete the installation:
   * **Runtime name**: The name of your runtime, starting with a lower-case character, and including up to 63 characters and numbers. Example: `csdpproduction`
	* **Select Kube context**: Your current context is highlighted. Press Enter to select it, or use the arrow keys to select a different context. 
	* **Repository URL**: The GitHub repo for the installation definitions, in the format `https://github.com/[repo_name]`. Example: `https:github.com/cf_production_install`
	* **Git provider API token**: The GitHub PAT for access to the GitHub repo.
	* **Ingress host (required)**: The external IP address or host name of the ingress host controller configured for your cluster.
	* **Install Codefresh demo resources?** Press Enter to confirm. Demo resources are saved in a new Git Source repo, created by CSDP. They include resources for two Hello World pipelines, one with a Cron trigger condition and the other with a Git event trigger condition.
	* **Do you wish to continue with runtime install?** Press Enter to confirm and start runtime installation.

When the runtime installed successfully message is displayed, go to the [**Runtimes**]((https://g.codefresh.io/2.0/account-settings/runtimes){:target="\_blank"}) dashboard in CSDP.  

### View runtime components and Git Sources
The **Runtimes** dashboard shows the runtime you just installed. You can drill down into the runtime to see its components and Git Sources. 
1. Select the runtime name to drill down.


  {% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/quick-start/quick-start-git-source-tab.png" 
   url="/images/getting-started/quick-start/quick-start-git-source-tab.png"
   alt="Git Source tab in runtime" 
   caption="Git Source tab in runtime"
   max-width="30%" 
   %} 






 The `cron/hello-world` pipeline shows statistics as it has already been triggered based on the cron interval.  
* The `githb/hello-world` pipelines has not been triggered as it requires a Git-event. As the final step of the runtime, you will trigger this pipeline.  

{% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/quick-start/quick-start-pipelines.png" 
   url="/images/getting-started/quick-start/quick-start-pipelines.png"
   alt="Demo pipelines after runtime imstall" 
   caption="Demo pipelines after runtime imstall"
   max-width="30%" 
%} 

### What to do next
[Trigger the demo Hello World pipeline]({{site.baseurl}}/docs/getting-started/quick-start-hello-world)