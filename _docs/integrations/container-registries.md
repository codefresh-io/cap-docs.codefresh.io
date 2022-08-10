---
title: "Docker registry integrations"
description: ""
group: integrations
toc: true
---

Codefresh can integrate with popular Docker container registries such as Docker Hub, JFrog Artifactory, and more.  

Adding a Docker registry integration in Codefresh allows you to reference the integration in external CI platforms/tools such as GitHub Actions and Codefresh Classic by the name of the integration, instead of explicit credentials. See [Image enrichment with integrations]({{site.baseurl}}/docs/integrations/image-enrichment-overview/) and [CI integrations]({{site.baseurl}}/docs/integrations/ci-integrations/).You can add more than one integration for the same Docker registry.  

You add a Docker registry integration in Codefresh by:
* Defining the integration name 
* Selecting the runtime or runtimes it is shared with
* Defining the arguments
* Testing the connection 
* Committing the changes

Once added, Codefresh displays the list of existing integrations with their sync status. You can edit or delete any registry integration. 


### Configure Docker registry integrations in Codefresh
Configure the settings for a Docker registry integration in Codefresh.

1. In the Codefresh UI, go to [Integrations](https://g.codefresh.io/2.0/account-settings/integrations){:target="\_blank"}.
1. Filter by **Docker Registry**, select the Docker registry, and click **Configure**.
1. If you already have integrations, click **Add**.
1. Define the arguments for the Docker registry: 
  [Amazon ECR]({{site.baseurl}}/docs/integrations/amazon-ecr/)   
  [Docker Hub]({{site.baseurl}}/docs/integrations/dockerhub/)   
  [JFrog Artifactory]({{site.baseurl}}/docs/integrations/jfrog/)  
  [Quay]({{site.baseurl}}/docs/integrations/jequaynkins/)
1. To test the connection to the Docker registry before committing the changes, click **Test Connection**.
1. To confirm, click **Commit**.
  It may take a few moments for the new integration to be synced to the cluster before it appears in the list.

### Integration resource in shared configuration repo
The integration resource is created in the Git repository with the shared configuration, within `resources`. The exact location depends on whether the integration is shared with all or specific runtimes:  

* All runtimes: Created in `resources/all-runtimes-all-clusters/`
* Selected runtimes: Created in `resources/runtimes/<runtime-name>/`

### View Docker registry integrations
Selecting a Docker registry integration displays the existing integrations for that registry.  
The example below shows integrations for JFrog Artifactory.  

{% include image.html 
lightbox="true" 
file="/images/jfrog/jfrog-int-list.png" 
url="/images/jfrog/jfrog-int-list.png"
alt="JFrog integrations in Codefresh"
caption="JFrog integrations in Codefresh"
max-width="50%"
%}

### Related articles
[Shared configuration repo]({{site.baseurl}}/docs/reference/shared-configuration/)  
[Images]({{site.baseurl}}/docs/deployment/images/)  
[Applications dashboard]({{site.baseurl}}/docs/deployment/applications-dashboard/)    
[Add Git sources to runtimes]({{site.baseurl}}/docs/runtime/git-sources/)  
