---
title: "WIP: 3. Install runtime"
description: ""
group: getting-started
toc: true
---


Installing the runtime installs the Codefresh Software Development Platform (CSDP), comprising Argo project components and CSDP-specific components. The Argo CD components are derived from a conformed fork of the Argo ecosystem, giving you an enterprise-supported version. 


### Before you begin
* [(Optional) Create a user account]({{site.baseurl}}/docs/getting-started/quick-start-add-user)
* Have your GitHub Personal Authentication Token (PAT) ready with a valid expiration date and `repo` access. 

  {% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/github-pat.png" 
   url="/images/getting-started/github-pat.png" 
   alt="GitHub PAT permissions" 
   caption="GitHub PAT permissions"
   max-width="30%" 
   %}  

See [GitHub article](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

### Download the CSDP CLI
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
For the quick start, you will install the runtime through the CLI. 

1. To start runtime installation, run `cf runtime install`.
1. Follow the prompts in the CLI wizard to complete the installation:
   * **Runtime name**: The name of your runtime, starting with a lower-case character, and including up to 64 characters and numbers. Example: `csdpproduction`
	* **Select Kube context**: Your current context is highlighted. Press Enter to select it, or use the arrow keys to select a different context. 
	* **Repository URL**: The GitHub repo for the installation definitions, in the format `https://github.com/[repo_name]`. Example: `https:github.com/cf_production_install`
	* **Git provider API token**: The GitHub PAT for access to the GitHub repo.
	* **Ingress host (required)**: The external IP address or host name of the ingress host controller configured for your cluster.
	* **Install Codefresh demo resources?** Press Enter to confirm. Demo resources are saved in a new Git Source repo, created by CSDP. They include resources for two Hello World pipelines, one with a Cron trigger and the other with a Git event as the trigger.
	* **Do you wish to continue with runtime install?** Press Enter to confirm and start runtime installation.

When the runtime installed successfully message is displayed, go to the [**Pipelines**]((https://g.codefresh.io/2.0/pipelines){:target="\_blank"}) page in CSDP.  

* The `cron/hello-world` pipeline shows statistics as it has already been triggered based on the cron interval.  
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

### Trigger Git event-based demo pipeline
In the Git Source repo containing the demo resources, commit a change `event-source.git-source.yaml` to trigger `githb/hello-world`.
1. Go to the [**Runtimes**]((https://g.codefresh.io/2.0/account-settings/runtimes){:target="\_blank"}) page in CSDP. 
1. Drill down into the runtime to view Runtime components and Git Sources.
  
  {% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/quick-start/quick-start-git-source-tab.png" 
   url="/images/getting-started/quick-start/quick-start-git-source-tab.png"
   alt="Git Source tab in runtime" 
   caption="Git Source tab in runtime"
   max-width="30%" 
   %} 

{:start="3"} 
1. Select **Go to repo**.
  
  {% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/quick-start/quick-start-download-cli.png" 
   url="/images/getting-started/quick-start/quick-start-download-cli.png" 
   alt="Example Git Hub repo with demo resources" 
   caption="Example Git Hub repo with demo resources"
   max-width="30%" 
   %} 

{:start="4"}  
1. Edit `event-source.git-source.yaml` and commit the change.   
1. Return to the **Pipelines** page. You will now see statistics for `githb/hello-world`.


### What to do next
[Create your pipeline]({{site.baseurl}}/docs/getting-started/quick-start-pipeline)