---
title: "Git Source management"
description: ""
group: runtime
toc: true
---

A Git Source links to a Git repository that stores resources for Codefresh runtimes, and CI/CD entities such as delivery pipelines, Workflow Templates, workflows, and applications. The Git Source syncs resources for Argo Workflows and Argo Events defined by the CI/CD entities to the cluster.  
  
Provisioning a runtime automatically creates a Git Source that stores resources for the runtime and for the demo CI pipelines that are optionally installed with the runtime. Every Git Source is associated with a Codefresh runtime. A runtime can have one or more Git Sources. You can add Git Sources at any time, to the same or to different runtimes.  

### View Git Sources
Git Sources are displayed when you drill down on a runtime in List View. 

1. In the CSDP UI, go to the [Runtimes](https://g.codefresh.io/2.0/account-settings/runtimes){:target="\_blank"} page.
1. In **List View** (the default), select a runtime name, and then select the **Git Sources** tab.  

  {% include 
	image.html 
	lightbox="true" 
	file="/images/runtime/git-source-list.png" 
	url="/images/runtime/git-source-list.png" 
	alt="Git Sources in runtime" 
	caption="Git Sources in runtime"
    max-width="30%" 
%}

{:start="3"}
1. To go to the repo linked to the Git Source, hover over the repo name in the Repo column, and select **Go to Git repo**.

### Create a Git Source
Create Git Sources for any provisioned runtime in List View.  

1. In the CSDP UI, go to the [Runtimes](https://g.codefresh.io/2.0/account-settings/runtimes**){:target="\_blank"} page.
1. In the List View, select the runtime for which to add a Git Source, and select **Create Git Sources**.

  {% include 
	image.html 
	lightbox="true" 
	file="/images/runtime/create-git-source.png" 
	url="/images/runtime/create-git-source.png" 
	alt="Create Git Source" 
	caption="Create Git Source"
    max-width="30%" 
%}

{:start="3"}
1. In the Create Git Source panel, define the requirements to create the Git Source:
  * **Name**: The Git Source must be unique within the cluster.
  * **Source**: The Git repo to which to commit the resources for this Git Source.
  * **Destination**: The namespace in the cluster to which to deploy the ???

### Edit a Git Source
Edit an existing Git Source by the source and destination definitions.  
> You cannot change the name of the Git Source.

1. In the CSDP UI, go to the [Runtimes](https://g.codefresh.io/2.0/account-settings/runtimes**){:target="\_blank"} page.
1. In the List View, select the runtime and then the **Git Sources** tab.
1. In the row with the Git Source to edit, click the three dots, and then select **Edit** in the panel that appears.

{% include 
	image.html 
	lightbox="true" 
	file="/images/runtime/edit-git-source.png" 
	url="/images/runtime/edit-git-source.png" 
	alt="Edit Git Source" 
	caption="Edit Git Source"
    max-width="30%" 
%}
1. Change the definitions for the Git Source, and select **Save**. 



### What to read next
[Manage runtimes]({{site.baseurl}}/docs/runtime/monitor-manage-runtimes/)
