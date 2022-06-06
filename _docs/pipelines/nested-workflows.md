---
title: "Nested workflows"
description: ""
group: pipelines
toc: true
---

Define single or multiple nested workflows within a workflow. A nested workflow is a step within the parent workflow that either submits a new workflow or creates a PR (Pull Request) that runs a different workflow based on the PR result.  

Nested workflows run independently of the parent workflow that submitted them. A nested submit workflow has traceability in both directions, from the parent to child, and from the child to the parent. A workflow triggered by a nested PR identifies the PR that triggered it. 

Codefresh has two ready-to-use Workflow Templates to:
* Submit a workflow
* Create a PR to run the workflow that tracks the PR
 
 

### Select the Codefresh Workflow Template

  Select the template you need either from Workflow Templates, where you can run it before submit, or from the Delivery Pipelines.  
 

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



### Navigate to parent/child workflows

Once the parent workflow is submitted, the Summary tab of the step in the parent or child workflow illustrates the nested relationship. 
* Submit workflow template: In the parent workflow, the nested-workflow step has a link to the child workflow. In the child workflow, the step has a link to the parent template.
* Create PR template: The child workflow indicates that it was triggered by the PR request. 
  
> To navigate to the Workflows tab with step visualizations, select the workflow and then **View workflow details**.  

     
**Example: Submit template - parent workflow with link to child workflow** 

      {% include 
   image.html 
   lightbox="true" 
   file="/images/workflows/nested-example-parent-submit.png" 
   url="/images/workflows/nested-example-parent-submit.png" 
   alt="Parent workflow with link to nested submit workflow" 
   caption="Parent workflow with link to nested submit workflow"
   max-width="50%" 
   %}

**Example: Submit template - child workflow with link to parent**
     
   {% include 
   image.html 
   lightbox="true" 
   file="/images/workflows/nested-example-child-submit.png" 
   url="/images/workflows/nested-example-child-submit.png" 
   alt="Child submit workflow with link to parent workflow" 
   caption="Child submit workflow with link to parent workflow"
   max-width="50%" 
   %}

**Example: Create PR template: - child workflow triggered by PR**
      
  {% include 
   image.html 
   lightbox="true" 
   file="/images/workflows/nested-example-pr.png" 
   url="/images/workflows/nested-example-pr.png" 
   alt="Workflow triggered by PR in parent workflow" 
   caption="Workflow triggered by PR in parent workflow"
   max-width="50%" 
   %}


 