---
title: "Nested workflows in Codefresh"
description: ""
group: pipelines
toc: true
---

Create pipelines that execute nested workflows, workflows that call other workflows. Codefresh supports two types of nested workflows:
1. Workflow that launches a child workflow
1. Workflow that waits for a Pull Request 

  pipelines by calling other pipelines from within an existing pipeline. This is easily accomplished with the codefresh-run plugin that allows you to launch another pipeline and optionally wait for its completion

  1. Workflow Template for pipeline
    When selecting the Workflow Template for the pipeline, go to Codefresh Hub for Argo and select one of these Workflow Templates:
     SCREENSHOT

  1. Customize the Workflow Template
     Customize the Workflow Template with the ???

  1. Identify step for parent or child workflows 
    Parent-child workflow steps
     When you visualize the workflow execution details, filter by Node type `Workflow` which is a custom Codefresh node type to jump to the relevant step. If you are in the parent workflow, the step links to the child workflow. If it is the child workflow, the step links to the parent workflow. 

     PR-based workflow steps



 