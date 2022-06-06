---
title: "Nested workflows in Codefresh"
description: ""
group: pipelines
toc: true
---

Define single or multiple nested workflows within a workflow. A nested workflow is a step within the parent workflow that either submits a new workflow or creates a PR (Pull Request) that runs a different workflow when merged. Nested workflows run independently of the parent workflow that submitted them. If you submitted a new workflow as a nested workflow, you have traceability in both directions, from the parent to child, and from the child to the parent. In case of nested PRs, the workflow triggered by the PR indicates this.  

Codefresh has two ready-to-use Workflow Templates to:
* Submit a workflow
* Create a PR to run the workflow that tracks the PR
 
 
**Get up and running with nested workflows**  

1. Select the Codefresh templates. 
  Select it from Workflow Templates, where you can run it before submit.  

  Select it as part of the Delivery Pipeline.  
 

   {% include 
   image.html 
   lightbox="true" 
   file="/images/workflows/nested-submit-termnate-template.png" 
   url="/images/workflows/nested-submit-termnate-template.png" 
   alt="Codefresh Workflow Template with nested submit" 
   caption="Codefresh Workflow Template with nested submit"
   max-width="50%" 
   %}


    {% include 
   image.html 
   lightbox="true" 
   file="/images/workflows/nested-pr-template.png" 
   url="/images/workflows/nested-pr-template.png" 
   alt="Codefresh Workflow Template with nested PR" 
   caption="Codefresh Workflow Template with nested PR"
   max-width="50%" 
   %}



1.  Trace/navigate to parent/child workflows  

    Once the parent workflow is submitted, a look at the Summary tab of the step in the parent or child workflow illustrates the nested relationship. 
    * Submit workflow template: In the parent workflow, the nested-workflow step has a link to the child workflow. In the child workflow, the step has a link to the parent template.
    * Create PR template: The child workflow indicates that it was triggered by the PR request. 
  
    > To navigate to the Workflows tab with step visualizations, select **View workflow details**.  

     
     **Example: Submit template - parent workflow with link to child workflow**
    SCREENSHOT

     **Example: Submit template - child workflow with link to parent**
     SCREENSHOT

     **Example: Create PR template: - child workflow triggered by PR**
      SCREENSHOT


 