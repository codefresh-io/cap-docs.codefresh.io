---
title: "Quay.io"
description: "Connect Quay registries with pipeline integration"
group: integrations
sub_group: docker-registries
redirect_from:
  - /docs/quayio/
  - /docs/docker-registries/external-docker-registries/quay-io/
toc: true
---
To configure Quay.io first select **Other Registries** from the new registry drop down and then provide the following:

* Registry Name - a unique name for this configuration.
* Username - your Quay.io username.
* Password - your Quay.io encrypted password.
* Domain - quay.io.

{% include image.html 
	lightbox="true" 
	file="/images/integrations/docker-registries/add-quay-registry.png" 
	url="/images/integrations/docker-registries/add-quay-registry.png" 
	alt="Add Quay.io Docker Registry" 
	max-width="60%" %}

## Related articles
[Working with Docker Registries]({{site.baseurl}}/docs/ci-cd-guides/working-with-docker-registries/)  
[Push step]({{site.baseurl}}/docs/pipelines/steps/push/)  
[Building and pushing an image]({{site.baseurl}}/docs/yaml-examples/examples/build-and-push-an-image/)  