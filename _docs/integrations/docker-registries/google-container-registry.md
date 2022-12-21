---
title: "Google Container Registry (GCR)"
description: "Use GCR with pipeline integrations"
group: integrations
sub_group: docker-registries
redirect_from:
  - /docs/google-cloud-registry/
  - /docs/docker-registries/external-docker-registries/google-container-registry/
toc: true
---
To configure GCR first select **Google Container Registry** from the new registry drop down and then provide the following:

* Registry Name - A unique name for this configuration
* Key File - The contents of a JSON key file. See below for [instructions](#section-generating-a-json-key-file)

{% include image.html 
	lightbox="true" 
	file="/images/integrations/docker-registries/add-gcr-registry.png" 
	url="/images/integrations/docker-registries/add-gcr-registry.png" 
	alt="Add Google Container Registry" 
	max-width="60%" %}

## Generating a JSON key file
The JSON key file holds your credentials for a given [Service Account](https://cloud.google.com/compute/docs/access/service-accounts). To generate your key file follow these instructions:

1. Go to your [Cloud Platform Console Credentials page](https://console.cloud.google.com/apis/credentials).
2. Select the project that you're creating credentials for.
3. To set up a new service account, click **Create credentials** and then select Service account key.
4. Choose the service account to use for the key.
5. Choose to download the service account's public/private key as a JSON file.

You can find the complete guide [here](https://support.google.com/cloud/answer/6158849#serviceaccounts).

## Working with multiple projects

If you have more than one repositories/projects in Google cloud, you can connect multiple GCR registries and define one as the "primary" for the gcr.io domain.

This means that every time Codefresh needs to pull an image it will use that integration. If you wish to use another project for pulling images,
you can use the `registry_context` property as described in [working with multiple registries]({{site.baseurl}}/docs/ci-cd-guides/working-with-docker-registries/#working-with-multiple-registries-with-the-same-domain).


## Related articles
[Working with Docker Registries]({{site.baseurl}}/docs/ci-cd-guides/working-with-docker-registries/)  
[Push step]({{site.baseurl}}/docs/pipelines/steps/push/)  
[Building and pushing an image]({{site.baseurl}}/docs/yaml-examples/examples/build-and-push-an-image/)  