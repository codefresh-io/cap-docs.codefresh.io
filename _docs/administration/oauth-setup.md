---
title: "Set up OAuth integration"
description: ""
group: administration
toc: true
---


Codefresh administrators can create, register and authorize Codefresh as an OAuth2 App in GitHub. Codefresh users can then authorize access to GitHub instead of generating a personal access token from their Git providers to perform Git-based actions.

To set up OAuth2 authorization for GitHub in Codefresh, you must:
* Create a GitHub OAuth2 Application for Codefresh 
* Create a K8s `secret` in the runtime cluster with OAuth2 Application credentials
* Configure OAuth2 settings in Codefresh to create a K8s `ConfigMap` that references the secret

### Step 1: Create GitHub OAuth2 Application
Create and register an OAuth App under your organization to authorize Codefresh.  

> Make sure your OAuth app has `repo` scope with write access to code. For more information, see [Scopes for OAuth apps](https://docs.github.com/en/developers/apps/building-oauth-apps/scopes-for-oauth-apps){:target="\_blank"}.   

1. Follow the step-by-step instructions in [GitHub](https://docs.github.com/en/developers/apps/building-oauth-apps/creating-an-oauth-app){:target="\_blank"}.   
  For the `Authorization callback URL`, enter this value:  
    `<ingressHost>/app-proxy/api/git-auth/github/callback`  
    where:  
    `<ingressHost>` is the IP address or URL of the ingress host in the runtime cluster. 
1. Make sure **Enable Device Flow** is _not_ selected. 
1. Select **Register application**. 
   The client ID is automatically generated, and you are prompted to generate the client secret.
1. Select **Generate a new client secret**, and copy the generated secret. 
1. Note down the following:
  * Application ID from the URL
  * Client ID and the client secret  

  You will need them to create the K8s secret for GitHub OAuth2 application.

### Step 2: Create a K8s secret resource in the runtime cluster 
Create a K8s secret in the runtime cluster, using the example below as a guideline. You need to define the application ID (`appId`), client ID (`clientId`) and the client secret (`clientSecret`) from the GitHub OAuth2 Application you created, and the GitHub URL (`url`).  

> All fields in the secret _must be_ encoded in `base64`.  
  To encode, use this command: `echo -n VALUE | base64`.  


**Before you begin**
Make sure you have the following handy:
* Application ID from the application's URL
* Client ID 
* Client secret
* GitHub URL


1. Create the manifest for the K8s secret resource.

```yaml
apiVersion: v1
kind: Secret
type: Opaque
metadata:
  name: github-oauth2
  namespace: <RUNTIME_NAME> # replace with the name of the runtime
  labels:
    codefresh_io_entity: git-pat-obtainer-sec
data:
  appId: # application ID of your OAuth app from GitHub
  clientId: # client ID of your OAuth app from GitHub
  clientSecret: # client secret of your OAuth app from GitHub
  url: https://github.com # GitHub provider URL which by default is github.com, unless self-hosted provider
```

{:start="2"}
1. Apply the secret to the runtime cluster:  
   `kubectl apply -f <filename>`   
   

### Step 3: Configure OAuth2 settings in Codefresh 

Configure the settings for the OAuth2 GitHub application in Codefresh to complete the integration. Configuring the settings will create a K8s config map that references the OAuth secret credentials. When configuring the settings, you can work in Form mode, or directly edit the YAML manifest. 

>Important:  
  > The values for all the settings in the config-map are the `keys` in the secret file. 

1. In the Codefresh UI, go to Authentication (TBD).
1. Select **GitHub Authentication**.
  The list shows existing GitHub authentications, if any. 

  SCREENSHOT

{:start="3"}
1. Select **+ Add**.
  The settings page is opened in the **Form** mode.
  
  SCREENSHOT

{:start="4"}
1. From the **Runtime** list, select the runtime to which to apply the current configuration. The runtime must be identical to the runtime to which you saved the K8s secret.
   > If you have managed clusters registered to the selected runtime, the configuration is available to all the clusters. 

1. Configure the settings for the **GitHub OAuth2 Application**, either in **Form** or in **YAML** modes:
  * **Secret Name**: The name of the K8s secret file you created in the runtime cluster.
  * **Secret Namespace**: The namespace in the runtime cluster where you created the K8s secret.
  * **Application ID**: The `key` representing the OAuth application ID in the K8s secret. For example, `appId`.
  * **Client ID**: The `key` representing the client ID in the K8s secret. For example, `clientId`.
  * **Client Secret**: The `key` representing the client secret in the K8s secret. For example, `clientSecret`.
  * **URL**: The `key` representing the provider URL in the K8s secret. For example, `url`.

  Here are examples of the OAuth2 settings in Form mode and the corresponding YAML file that Codefresh automatically populates.

  SCRESSNHOT

{:start="5"}
1. Select **Commit**.
  The Commit Changes panel shows a summary of the settings and the final version of the YAML manifest in read-only mode. 
  
  SCREENSHOT

{:start="6"}  
1. From the **Select Git Source** list, select the Git Source in which to store the manifest for the `ConfigMap` you are creating.
  The list displays all the Git Sources created for the selected runtime. 
1. Optional. Enter a commit message.
1. Select **Commit** at the bottom-right.

You have completed the setup for authorizing Codefresh as an OAuth App in GitHub. 

