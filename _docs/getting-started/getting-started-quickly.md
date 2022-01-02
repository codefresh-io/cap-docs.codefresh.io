---
title: "Getting started quickly"
description: ""
group: getting-started
toc: true
---

Follow the steps to get up and running in the CSDP platform, starting with creating a user account, installing CSDP and creating a pipeline.

### 1. Verify system requirements
Review and confirm that your deployment conforms to the minimum requirements for CDP

### 2. Create a user account
You will create a new user account with the `admin` role.
##### How to
1. In CSDP, click **Account Settings**.
1. From the sidebar, select [Collaboration]((https://g.codefresh.io/2.0/account-settings/users){:target="\_blank"}).  
   {% include
   image.html
   lightbox="true"
   file="/images/administration/users/users-list.png"
   url="/images/administration/users/users-list.png"
   alt="Users list"
   caption="Users list"
   max-width="50%"
   %}
{:start="3"} 
1. Select **Users**, and then select **+ [Add User]**.

   {% include 
   image.html 
   lightbox="true" 
   file="/images/administration/users/invite-user.png" 
   url="/images/administration/users/invite-user.png" 
   alt="Add new user" 
   caption="Add new user"
   max-width="50%" 
   %}  

   1. Type the **User's email address**.  
   1. **Assign a role**, by selecting **Administrator**.


### 3. Install the CSDP runtime
Installing the runtime installs the Codefresh Software Development Platform (CSDP), comprising Argo CD components and CSDP-specific components. The Argo CD components are derived from a conformed fork of the Argo ecosystem, giving you an enterprise-supported version.  

##### Before you begin
Have you GitHub Personal Authentication Token ready

##### How to
1. In the Welcome page, select **+ Install Runtime**.
1. Download the Codefresh CLI:
  * Select one of the methods. 
    *  Mac: ```brew tap codefresh-io/cli && brew install cf2```
    * Linux x86: ```curl -L --output - https://github.com/codefresh-io/cli-v2/releases/latest/download/cf-linux-amd64.tar.gz | tar zx && mv ./cf-linux-amd64 /usr/local/bin/cf && cf version```
    * Linux ARM: ```curl -L --output - https://github.com/codefresh-io/cli-v2/releases/latest/download/cf-linux-arm64.tar.gz | tar zx && mv ./cf-linux-arm64 /usr/local/bin/cf && cf version```
   * Or for windows, download from https://github.com/codefresh-io/cli-v2/releases/latest/download/cf-windows-amd64.tar.gz
  * Generate the API key and create the authentication context. 
1. Install the runtime:
  * Run: `cf runtime install`.
  * Follow the prompts in the CLI wizard to complete the installation:
    * **Runtime name**: The name of your runtime, starting with a lower-case character, and including up to 64 characters and numbers. Example: `cfproduction`
	* **Select Kube context**: Your current context is highlighted. Press Enter to select it. 
	* **Repository URL**: The GitHub repo for the installation definitions, in the format `https://github.com/[repo_name]`. Example: `https:github.com/cf_production_install`
	* **Git provider API token**: The GitHub PAT for access to the GitHub repo.
	* **Ingress host (required)**: The external IP address/host name/FQDN of the ingress host controller configured for your cluster.
	* **Install Codefresh demo resources?** Press Enter to confirm installing 
	* **Do you wish to continue with runtime install? Press Enter to confirm and start runtime installation.


### 4. Create a pipeline
TBD
