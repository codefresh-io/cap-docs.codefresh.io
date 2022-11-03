---
title: "Set up OAuth2 authentication"
description: ""
group: administration
toc: true
---

Easily Connect Codefresh to your Git provider using OAuth2 for authorization. 
Codefresh integrates with Git to sync repositories to your clusters, implementing Git-based operations when creating resources such as Delivery Pipelines, and to enrich Images with valuable information.


Codefresh supports OAuth2 or personal access tokens (PATs) for authentication:

* OAuth2 with Codefresh OAuth Application or custom OAuth2 Application

  OAuth2 is the preferred authentication mechanism, supported for popular Git providers such as GitHub, GitHub Enterprise, GitLab Cloud and Server, and Bitbucket Cloud and Server. You have the option to use the default pre-defined Codefresh OAuth Application, or your own Oauth2 Application in your Git account. 
  To use your own Oauth2 GitHub Application, create a secret on your K8s cluster, and configure OAuth2 access in Authentication > Settings. See [Create a custom OAuth2 provider account](#create-a-custom-oauth2-provider-account) in this article.
  

  > A runtime can have only one active account for authentication. 

* Token-based authentication using PAT  
  With token-based authentication, users must generate personal access tokens from their Git providers with the required scopes and enter their personal access tokens when prompted to authorize access. Token-based authentication for users is described in [Authorize Git access in Codefresh]({{site.baseurl}}/docs/administration/user-settings/#authorize-git-access-in-codefresh).


### Authentication provider accounts
The authentication accounts created for a provider are displayed in [Authentication](https://g.codefresh.io/2.0/account-settings/authentication?providerName=github){:target="\_blank"}.  

{% include 
   image.html 
   lightbox="true" 
   file="/images/authentication/authentication-list.png" 
   url="/images/authentication/authentication-list.png" 
   alt="Git provider authentication accounts" 
   caption="Git provider authentication accounts"
   max-width="60%" 
   %}
The authentication accounts are organized by Runtimes. Every runtime can have a single authentication account.   
The Type column identifies the provider account type as either Codefresh or Custom. You can change the provider type to Codefresh or Custom or select manual token entry as the authentication method.  


### Create a custom OAuth2 provider account 
Codefresh account administrators can create an OAuth2 Application in their Git provider accounts, and set up authentication within Codefresh. Users in Codefresh can then authorize access to the Git provider using OAuth2, instead of a personal access token.  

Supported Git providers:
* GitHub and GitHub Enterprise
* GitLab Cloud and GitLab Server
* Bitbucket Cloud (hosted) and Bitbucket Server (hybrid)

To set up OAuth2 authorization for GitHub in Codefresh, you must:
* [Create OAuth2 Application for Git provider](#create-oauth2-application-for-git-provider) 
* [Create a K8s `secret` in the runtime cluster with OAuth2 Application credentials]
* Configure OAuth2 settings in Codefresh to create a K8s `ConfigMap` that references the secret

#### Step 1: Create OAuth2 Application for Git provider
Create and register an OAuth App under your organization to authorize Codefresh.  

> Make sure your OAuth app has `repo` scope with write access to code. For more information, see [Scopes for OAuth apps](https://docs.github.com/en/developers/apps/building-oauth-apps/scopes-for-oauth-apps){:target="\_blank"}.   

1. Follow the step-by-step instructions for your Git provider:
  **[GitHub]**(https://docs.github.com/en/developers/apps/building-oauth-apps/creating-an-oauth-app){:target="\_blank"}
     Notes:  
     * For **Authorization callback URL**, enter this value:  
       `<ingressHost>/app-proxy/api/git-auth/github/callback`  
       where:  
       `<ingressHost>` is the IP address or URL of the ingress host in the runtime cluster.
     * Make sure **Enable Device Flow** is _not_ selected. 
     * Select **Register application**. 
       The client ID is automatically generated, and you are prompted to generate the client secret.
     * Select **Generate a new client secret**, and copy the generated secret.  

   **[GitLab Cloud and Server]**(https://docs.gitlab.com/ee/integration/oauth_provider.html#user-owned-applications){:target="\_blank"} 
    * For **Redirect URI**, enter this value:  
      `<ingressHost>/app-proxy/api/git-auth/gitlab/callback`  
      where:  
      `<ingressHost>` is the IP address or URL of the ingress host in the runtime cluster.

  **[Bitbucket Cloud and Server]**(https://support.atlassian.com/bitbucket-cloud/docs/use-oauth-on-bitbucket-cloud/){:target="\_blank"}
    * For **Callback URL**, enter this value:  
      `<ingressHost>/app-proxy/api/git-auth/bitbucket-server/callback`  
      where:  
      `<ingressHost>` is the IP address or URL of the ingress host in the runtime cluster.

1. Note down the following, as you will need them to create the K8s secret for the Git OAuth2 application:
  * GitHub: Application ID from the URL, Client ID, and the client secret  
  * GitLab Cloud and Server: Application ID and Secret
  * Bitbucket Server: Key and Secret



#### Step 2: Create a K8s secret resource in the runtime cluster 
Create a K8s secret in the runtime cluster, using the example below as a guideline. You must define the application ID (`appId`), client ID (`clientId`) and the client secret (`clientSecret`) from the OAuth2 Application you created in your Git provider, and the Git URL (`url`).  

> All fields in the secret _must be_ encoded in `base64`.  
  To encode, use this command: `echo -n VALUE | base64`.  


**Before you begin**  

Make sure you have the following handy:
* GitHub: Application ID from the URL, Client ID, and the client secret  
* GitLab Cloud and Server: Application ID and Secret
* Bitbucket Server: Key and Secret


**How to**  

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
  appId: # application ID of your OAuth app
  clientId: # client ID of your OAuth app 
  clientSecret: # client secret of your OAuth app
  url: https://github.com # Git provider URL which by default is github.com, unless self-hosted provider
```

{:start="2"}
1. Apply the secret to the runtime cluster:  
   `kubectl apply -f <filename>`   
   

#### Step 3: Configure OAuth2 settings in Codefresh 

To complete custom provider authentication, configure the settings for the OAuth2 Git application in Codefresh. Configuring the settings creates a K8s ConfigMap that references the OAuth secret credentials. When configuring the settings, you can work in Form mode, or directly update the YAML manifest. 

>Important:  
  > The values for all the settings in the ConfigMap are the `keys` in the secret file. 

1. In the Codefresh UI, go to [Authentication](https://g.codefresh.io/2.0/account-settings/authentication?providerName=github){:target="\_blank"}.
  The list always shows the default predefined Codefresh provider account and custom provider accounts created, organized by Runtime, Type (Codefresh or Custom) and Status. 
1. From the list, select the runtime to which to apply the current configuration. 
   >The runtime must be identical to the runtime to which you saved the K8s secret.
1. Click **Edit** and then select **Use custom provider**.
   > If you have managed clusters registered to the selected runtime, the authentication account is available to all the clusters.  
  The settings page is opened in **Form** mode.
    
{% include 
   image.html 
   lightbox="true" 
   file="/images/authentication/oauth-custom-settings.png" 
   url="/images/authentication/oauth-custom-settings.png" 
   alt="OAuth settings for custom provider in Codefresh" 
   caption="OAuth settings for custom provider in Codefresh"
   max-width="50%" 
   %}

{:start="4"}
1. Configure the settings for the **Git OAuth2 Application**, either in **Form** or in **YAML** modes:
  * **Secret Name**: The name of the K8s secret file you created in the runtime cluster.
  * **Secret Namespace**: The namespace in the runtime cluster where you created the K8s secret.
  * **Application ID**: The `key` representing the OAuth application ID in the K8s secret. For example, `appId`.
  * **Client ID**: The `key` representing the client ID in the K8s secret. For example, `clientId`.
  * **Client Secret**: The `key` representing the client secret in the K8s secret. For example, `clientSecret`.
  * **URL**: The `key` representing the Git provider URL in the K8s secret. For example, `url`.

{:start="5"}
1. Click **Commit**.
  The Commit Changes panel shows a summary of the settings and the final version of the YAML manifest in read-only mode. 
  
{% include 
   image.html 
   lightbox="true" 
   file="/images/authentication/oauth-custom-commit-settings.png" 
   url="/images/authentication/oauth-custom-commit-settings.png" 
   alt="OAuth settings for custom provider in Codefresh" 
   caption="OAuth settings for custom provider in Codefresh"
   max-width="50%" 
   %}

{:start="6"}  
1. From the **Select Git Source** list, select the Git Source in which to store the manifest for the `ConfigMap` you are creating.
  The list displays all the Git Sources created for the selected runtime. 
1. Optional. Enter a commit message.
1. At the bottom-right, click **Commit** once again.

You have completed the setup to authorize Codefresh as an OAuth App for your Git provider. 

### Select authentication account for runtime
For a Git provider and a runtime account, select the authentication mechanism: Codefresh account, Custom provider account if one exists, or token-based authentication.

1. In the Codefresh UI, go to [Authentication](https://g.codefresh.io/2.0/account-settings/authentication?providerName=github){:target="\_blank"}.
1. Select the runtime, and click ![](/images/administration/users/icon-Edit.png?display=inline-block) **Edit**. 
1. Select the OAuth authentication provider account.