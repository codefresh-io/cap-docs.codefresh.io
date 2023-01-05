---
title: "Create Docker registry integration"
description: ""
group: quick-start
toc: true
---

Connect you Docker regsitry to Codefresh to store the Docker image of the application you will create.
Codefresh supports all the popular Docker registries. 
If you don't already have a registry, we recommend starting with the GitHub Registry for this quick start. 

## Where are you

## Before you begin

## How to

1. In the Codefresh UI, on the toolbar, click the Settings icon, and then from Configuration, select **Pipeline Integrations**.
1. Select **Docker Registries** and then click **Configure**.
1. From the **Add Registry Provider** dropdown, select **Other Registries**.
1. Define the following:  
  * **Registry name**: A unique name for this configuration.
  * **Username**: Your GitHub username.
  * **Password**: Your GitHub personal token.
  * **Domain**: `ghcr.io`.
  * Expand **Advanced Options** and define the [**Repository Prefix**]({{site.baseurl}}/docs/integrations/docker-registries/#using-an-optional-repository-prefix) as your GitHub username.

{% include image.html 
	lightbox="true" 
	file="/images/integrations/docker-registries/github/github-registry-codefresh.png" 
	url="/images/integrations/docker-registries/github/github-registry-codefresh.png" 
	alt="GitHub Container Registry settings"
	caption="GitHub Container Registry settings" 
	max-width="70%" 
%}

{:start="5"}
1. To verify the connection details, click **Test Connection**.
1. To apply the changes, click **Save**.

## Continue with 
