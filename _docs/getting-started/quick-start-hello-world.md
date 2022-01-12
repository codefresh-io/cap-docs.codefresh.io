---
title: "WIP: 3. Trigger the Hello World example pipeline"
description: ""
group: getting-started
toc: true
---

Now that you have successfully installed the CSDP runtime, you can trigger one of the Hello World demo pipelines included in the runtime package.
The two Hello World example pipelines are triggered by different event conditions:
* Calendar (cron) event
* Git (GitHub) event 

Let's focus for now on the `github/hello-world` pipeline.

### View pipelines
View the pipelines in CSDP. 
1. In the CSDP UI, go to [Pipelines]((https://g.codefresh.io/2.0/pipelines){:target="\_blank"}). 

  {% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/quick-start/quick-start-pipelines.png" 
   url="/images/getting-started/quick-start/quick-start-pipelines.png" 
   alt="Demo pipelines in the Pipelines page" 
   caption="Demo pipelines in the Pipelines page"
   max-width="30%" 
   %}  

 * The `cron/hello-world` pipeline shows statistics as it has already been triggered based on the `cron` interval.  
 * The `githb/hello-world` pipeline has not been triggered as it requires a Git event to trigger it. 

### View and update manifest
Because we don't have a workflow for this pipeline, you will configure the git Source reosurce in the pipeline's **Manifest**.
1. To drill down, select the pipeline name.
1. Select the **Manifest** tab, and click the arrowhead to expand the resource view.
  
   {% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/quick-start/quick-start-manifest-expand.png" 
   url="/images/getting-started/quick-start/quick-start-manifest-expand.png" 
   alt="Expand resource view in Mainfest tab" 
   caption="Expand resource view in Mainfest tab"
   max-width="30%" 
   %} 
  
  You can see these resources:    

  * Calendar EventSource (`event-source.git-source.yaml`)
  * Github Sensor (`sensor.git-source.yaml`)
  * Hello-world WorkflowTemplate (`workflow-template.hellow-world.yaml`)  


  > The pipeline is configured to run on a `PUSH` event in the Git repository.

{:start="3"}
1. Update the public URL for the Webhook event:
  * From the expanded resource manifest, select `event-source.git-source.yaml`.
  * Select **Edit** and scroll to line 29 in the resource file.    
  * Replace the placeholder with a valid URL that can be accessed by the cluster. Webhooks are sent to this URL.
  * Select **Commit** to commit your changes.  
  
     {% include 
    image.html 
   lightbox="true" 
   file="/images/getting-started/quick-start/quick-start-git-source-manifest-edit.png" 
   url="/images/getting-started/quick-start/quick-start-git-source-manifest-edit.png" 
   alt="Edit `event-source.git-source.yaml` in Manifest" 
   caption="Eid `event-source.git-source.yaml` in Manifest"
   max-width="30%" 
    %} 

CSDP does the following:
* Commits the changes to your Git repository.
* Synchronizes the changes in Git back to your cluster,  and updates the `event-source.git-source` resource.
* Triggers this pipeline after the `PUSH` event to your repository.
* Creates a workflow. View it in [Workflows]((https://g.codefresh.io/2.0/workflows){:target="\_blank"}).


### What to do next
[Install runtime]({{site.baseurl}}/docs/getting-started/quick-start-runtime)
