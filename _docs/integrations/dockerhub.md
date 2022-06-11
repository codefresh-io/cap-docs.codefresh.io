---
title: "Docker Hub Registry"
description: "Push, Pull and Deploy images from Dockerhub"
group: integrations
toc: true
---

Codefresh has native support for interacting with Dockerhub registries.

### Prerequisites
Before you configure settings in Codefresh to integrate Dockerhub registry, do the following:

* [Create an account or sign in to your account at Docker Hub](https://hub.docker.com/signup){:target="\_blank"}
* [Enable 2FA (Two-Factor Authentication)](https://docs.docker.com/docker-hub/2fa/){:target="\_blank"}
* [Create a personal account token](https://docs.docker.com/docker-hub/access-tokens/){:target="\_blank"}

## Configure Docker Hub integration in Codefresh
Once you have completed the prerequisites, configure the Docker Hub integration settings in Codefresh.  
 
1. In the Codefresh UI, go to [Integrations](https://g.codefresh.io/2.0/account-settings/integrations){:target="\_blank"}. 
1. Select **Docker Hub Docker Registry**, and then click **Add**.
1. Click **Add** on the top-right. 
1. Configure the Docker Hub integration settings: 
  * Enter an **Integration name**. You can have multiple Docker Hub instances connected.
  * Use this integration for **All runtimes**, or specific **Selected runtimes**.
  * **Username**: The Docker Hub username.
  * **Password**: 
    If you enabled two-factor authentication, enter the personal access token for your Docker Hub account for Codefresh to be able to push images. Personal access tokens are more secure and can be revoked when needed. Codefresh can then push your images.  
    If two-factor authentication is not enabled, enter the password of your Docker Hub account (not recommended).

    {% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/quick-start/quick-start-download-cli.png" 
   url="/images/getting-started/quick-start/quick-start-download-cli.png" 
   alt="Download CLI to install runtime" 
   caption="Download CLI to install runtime"
   max-width="30%" 
   %}
   
{:start="5"}
1. To confirm, click **Commit**.
  It may take a few moments for the new integration to be synced to the cluster before it appears in the list.

## What to read next
[Adding Git sources]({{site.baseurl}}/docs/runtime/git-sources/)










