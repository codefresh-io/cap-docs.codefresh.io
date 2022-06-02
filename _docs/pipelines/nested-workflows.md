---
title: "Nested workflows in Codefresh"
description: ""
group: pipelines
toc: true
---

Execute nested workflows from within Codefresh pipelines. Executed from within a parent workflow, the nested workflow runs a new workflow that is linked to but independent of the parent. Submit new workflows, terminate a referenced workflow, or create a Git PR (Pull Request) and reference workflow. Codefresh has created two specialized Workflow Templates that execute nested workflows in the Codefresh Hub for Argo.

**Benefits**
* The parent workflow continues execution regardless of the status of the child workflow.
* Complete traceability from the parent to the child, and from the child to the parent workflows
 
**Get up and running with nested workflows**  

1. Workflow Templates for pipeline
    When selecting the Workflow Template for the Delivery Pipeline, go to Codefresh Hub for Argo and select one of these Workflow Templates:  
  
    [Submit or Terminate workflows](https://codefresh.io/argohub/workflow-template/argo-workflows)  

    [Create PR Codefresh](https://codefresh.io/argohub/workflow-template/github)  
    
    {% include 
   image.html 
   lightbox="true" 
   file="/images/workflows/nested-pr-template.png" 
   url="/images/workflows/nested-pr-template.png" 
   alt="Create PR Codefresh Workflow Template" 
   caption="Create PR Codefresh Workflow Template"
   max-width="30%" 
   %}


1. Customize the Workflow Template
  Customize the Workflow Template according to the ReadMe files:  

    [Submit new workflow](https://github.com/codefresh-io/argo-hub/blob/main/workflows/argo-workflows/versions/0.0.3/docs/submit-workflow.md)  

    [Terminate workflow](https://github.com/codefresh-io/argo-hub/blob/main/workflows/argo-workflows/versions/0.0.3/docs/terminate-workflow.md)  

    [Create PR Template](https://github.com/codefresh-io/argo-hub/blob/main/workflows/github/versions/0.0.4/docs/create-pr-codefresh.md)

1.  Identify parent/child steps in workflows
    Once the pipeline is triggered and the parent workflow is submitted, the Summary tab of the workflow step links to the parent or child workflows, or identifies that the child workflow was triggered by a PR. 
    > To navigate to the Workflows tab with the step visualization , select **View workflow details**. 
     
     **Example: Parent workflow with link to child workflow**


     **Example: Child workflow with link to parent**
     

     **Example: Child workflow triggered by PR**



 