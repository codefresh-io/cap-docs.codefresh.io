---
title: "Azure Docker registry"
description: "Use the Azure Docker Registry for pipeline integrations"
group: integrations
sub_group: docker-registries
redirect_from:
  - /docs/docker-registries/external-docker-registries/azure-docker-registry/
toc: true
---
To configure the [Azure Docker registry](https://docs.microsoft.com/en-us/azure/container-registry/) within Codefresh, go into its settings in Azure Portal and select *Access Keys* from the left sidebar.

 {% include 
image.html 
lightbox="true" 
file="/images/integrations/docker-registries/azure-registry-admin.png" 
url="/images/integrations/docker-registries/azure-registry-admin.png" 
alt="Docker credentials for the Azure registry" 
caption="Docker credentials for the Azure registry" 
max-width="80%" 
%}

Click the *enable* button on the "Admin user" section. Change the username (optional) and make sure that you note down one of the passwords shown on the screen.

Then in the integration page in Codefresh, select **Other Registries** from the new registry drop down and then provide the following

* Registry Name - a unique name for this configuration.
* Username - your Azure Registry username.
* Password - your Azure Registry password.
* Domain - `<registry_name>.azurecr.io`.

{% include image.html 
	lightbox="true" 
file="/images/integrations/docker-registries/add-azure-registry.png" 
url="/images/integrations/docker-registries/add-azure-registry.png" 
alt="Adding the Azure Docker registry" 
caption="Adding the Azure Docker registry" 
max-width="60%" %}

Click the *Test* button to verify the settings and then the *Save* button to apply the changes.

## Using the Azure Registry

You can now use the Azure Registry in the pipelines either via the GUI  or with the by the YAML [push step]({{site.baseurl}}/docs/pipelines/steps/push/) (recommended).

It is also possible to use the registry from the command line with

```
docker login <registry_name>.azurecr.io -u <user_name> -p<password>
```

You can also inspect the pushed images either using Azure portal or with [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest)

```
az acr repository list --name <registry_name> --output table
```


## Related articles
[Working with Docker Registries]({{site.baseurl}}/docs/ci-cd-guides/working-with-docker-registries/)  
[Push step]({{site.baseurl}}/docs/pipelines/steps/push/)  
[Building and pushing an image]({{site.baseurl}}/docs/yaml-examples/examples/build-and-push-an-image/)  