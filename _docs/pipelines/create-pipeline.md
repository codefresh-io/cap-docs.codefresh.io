---
title: "Create pipelines"
description: ""
group: pipelines
toc: true
---

Delivery Pipelines are where all the magic happen in CSDP. Delivery pipelines connect Argo Workflows and Argo Events to implement and optimize CI/CD flows, such as checking out code, building and testing artifacts, run unit and acceptance tests, or any custom flow you define. 

CSDP builds on and integrates with Argo Workflows and Argo Events. Our Delivery Pipeline wizard brings the power of Argo Workflows and Argo Events, while greatly simplifying the process of pipeline creation, including trigger conditions, to generate workflows. 


### Delivery pipeline concepts
Let's start by reviewing main concepts of CSDP delivery pipelines - how we integrate with Argo Workflows and Argo Events, and what is unique functionality and enhancements.  

#### Pipeline per event type
The name of the delivery pipeline is created from the name of the sensor and the name of the trigger, which is the event type that triggers the pipeline.
A sensor-trigger-type pair is a unique pipeline in CSDP. The same sensor can be associated with different trigger types.

Each delivery pipeline links through a Git Source to a Git repository that houses the resources of the delivery pipeline. All commits to this pipeline's resources are made to the repo. The Git Source also determines which CSDP runtime executes the pipeline.



#### Argo Workflows
Fully integrated with Argo Workflows, CSDP implements Argo Workflows as a Kubernetes CRD (Custom Resource Definition).  

A delivery pipeline starts with the Workflow Template that defines the CI/CD flows implemented when the pipeline is triggered and workflows are generated.  
 
If you are looking for Workflow Template resources, both Argo and Codefresh have examples and libraries of Workflow Templates you can use.
* For conceptual information on Argo Workflows, read the [official documentation](https://argoproj.github.io/argo-workflows/){:target="\_blank"}.
* For examples of Workflow Templates in Argo, see their [documentation by example](https://github.com/argoproj/argo-workflows/blob/master/examples/README.md){:target="\_blank"} page.
* And of course, Codefresh offers a fully-certified and always expanding library of ready-to-use Workflow Templates in [Codefresh Hub for Argo](https://codefresh.io/argohub/){:target="\_blank"}.

In the pipeline wizard, we have our starter Workflow Template to use as a base, or you can copy and paste a predefined Workflow Template, and then modify as needed. 
> If your Workflow Template generates artifacts, you must [configure an artifact repository in CSDP]({{site.baseurl}}/docs/pipelines/configure-artifact-repository) to house the artifacts.

#### Arguments
A delivery pipeline has two sets of arguments, the globally-scoped arguments required as input parameters in the Workflow Template, and the arguments required by the sensor to trigger events.  

For ease of use and visibility, the **Arguments** tab in the delivery pipeline creation wizard exposes these globally-scoped input parameters in the Workflow Template. You can set the default values here.
{% include 
   image.html 
   lightbox="true" 
   file="/images/pipeline/create/pipeline-wizard-arguments-tab.png" 
   url="/images/pipeline/create/pipeline-wizard-arguments-tab.png" 
   alt="Arguments tab in delivery pipeline wizard" 
   caption="Arguments tab in delivery pipeline wizard"
   max-width="30%" 
   %} 

The arguments required by the Sensor are displayed in the Trigger Conditions tab, described in Argo Events below.  

The starting assumption is that the both sets of arguments are identical, and remain so. But if there are changes once the pipeline is triggered, CSDP records them in the Arguments tab, as the point of reference. The tag next to the argument indicates the type of change.   

`New`: Added to the Workflow Template, but not used by the sensor.  

`Not consumed`: Removed from the Workflow Template, but still used by the sensor.  

{% include 
   image.html 
   lightbox="true" 
   file="/images/pipeline/create/pipeline-arguments-modified.png" 
   url="/images/pipeline/create/pipeline-arguments-modified.png" 
   alt="Arguments tab showing changes" 
   caption="Arguments tab showing ch"
   max-width="30%" 
   %} 


#### Argo Events
As with Argo Workflows, CSDP supports Argo Events. What is different in CSDP is that there is no need to manually create and define sensor dependencies for Argo Events. The wizard guides you through selecting an event source, event type, and defining the trigger for each event.  

For conceptual information on Argo Events, read the [official documentation](https://argoproj.github.io/argo-events/){:target="\_blank"}.  

In the delivery pipeline wizard, **Trigger Conditions** collates all the requirements to set up and configure Argo Events and their dependencies for pipelines.  
Here, you select the event source, the type of event, the arguments to populate from the event payload, and filters to configure additional event dependencies. On commit, CSDP generates the manifests for the Workflow Template, the event source, and the sensor.

{% include 
   image.html 
   lightbox="true" 
   file="/images/pipeline/create/pipeline-wizard-trigger-tab.png" 
   url="/images/pipeline/create/pipeline-wizard-trigger-tab.png" 
   alt="Trigger Conditions tab in delivery pipeline wizard" 
   caption="Trigger Conditions tab in delivery pipeline wizard"
   max-width="30%" 
   %} 

**Git repos for event triggers**  
The code repositories for the event triggers. These code repos are different from the Git Source-linked repo where you commit pipeline resources, and which determines the runtime that executes the pipeline.

**Event sources and Event Types**
Currently we support GitHub as an Event Source, and an extensive list of GitHub event types you can select from for Continuous Integration.  

**Arguments**  
Every event type supports a set of arguments you can configure for that event. These arguments are identical to those referenced in the Workflow Template. As with Workflow Template-level arguments, you can add default values, or create definitions using predefined variables.  

When you use variable definitions, CSDP automatically maps the definition to the path in the manifest generated on commit. You don't need to remember or be aware of resource paths for event-type arguments as CSDP does it for you. Argo Workflows then extracts the value from the event payload at runtime. 

> If values are defined for an argument both in the Workflow Template and in Trigger Conditions, the value defined in Trigger Conditions takes precedence. 

**Filters**  
Filters create dependencies for event type triggers by enforcing validity constraints on what triggers an event. For example, add a filter to a Git push event that triggers that event only on push to a particular _branch_.   

CSDP supports Argo Events Data filters.

**Examples of common event types and filters**  

Let's review some examples of common Git events that trigger pipelines, and when you would use filters to further control when the Git events trigger the pipeline.

* Git commits which are push events 
* Git pull repuests (PRs), open PRs

The table below summarizes the use cases, event types and filters.

{: .table .table-bordered .table-hover}
|#|Trigger requirement                                               | Event type  |  Filter          |  
|--| --------------                                                | ---------------- | -----------|
|1| On Git commit                                                |`Commit pushed`      | None |
|2| On Git commit, _only_ when the commit is on a specific branch. This requires a filter to define the target branch.             | `Commit pushed`      | `Git branch`  = `<branch-name>`, for example, `main` |          
|3| On an opened PR                                                                   | `PR opened`          | None |                             
|4|On an opened PR, _only_ for a specific branch                                        | `PR opened`          | `PR target branch`  = `<branch-name>`, for example, `production` | 
|5|On an opened PR, _only_ for a specific branch, and _only_ when the PR name includes a predefined string|  `PR opened`      | `PR target branch` = `<branch-name>`, for example, `production` and `PR labels` = `<PR-name>`, for example, `cf-hotifx-xxxx`| 

Now let's review the use case in more detail.
#1 The `Commit pushed` event on any of the Git repos defined triggers the pipeline.  

#2 The `Commit pushed` event on any of the Git repos triggers the pipeline, _but only_ if the branch is `production` and not if the branch is `staging`.
  For example, only trigger if the commit is on a branch 

#3 The `PR opened` event triggers the pipeline when any PR is opened on any Git repo defined.  

#4 The `PR opened` event with the `PR Target Branch` filter triggers the pipeline _only_ when the target of a PR, that is, where the PR will be merged, matches the target branch name. You want to trigger the pipeline when the PR is opened on either `master` or `production`, but not when opened on `staging` or `qa`.

#5 The `PR opened` event with the `PR Target Branch` and `PR label` filters is a more complex scenario that triggers the pipeline when a PR is opened on the target branch, but only when the PR name starts with or includes a predefined value. Useful when collaborating on features, or when deploying a critical fix. Both the `PR Target Branch` and `PR label` must match for the pipeline to be triggered. The filters automatically excludes all PRs opened on the `production` branch but not matching the `PR label`. `hotfix-runtime`.


#### Manifests
Manifests are automatically generated on committing changes after defining at least one Trigger Condition. 
Manifests typically include:
* Sensor
* EventSource
* WorkflowTmplate

{% include 
   image.html 
   lightbox="true" 
   file="/images/pipeline/create/pipeline-wizard-resources.png" 
   url="/images/pipeline/create/pipeline-wizard-resources.png" 
   alt="Argo Event resources in delivery pipeline wizard" 
   caption="Argo Event resources in delivery pipeline wizard"
   max-width="30%" 
   %}

### Create a pipeline step-by-step
Follow our step-by-step instructions to guide you through the Delivery pipeline wizard

1. In the CSDP UI, go to [Delivery Pipelines](https://g.codefresh.io/2.0/pipelines){:target="\_blank"}.
1. Select **+ Add Delivery Pipeline**.

  {% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/quick-start/quick-start-new-pipeline.png" 
   url="/images/getting-started/quick-start/quick-start-new-pipeline.png" 
   alt="Add Delivery Pipeline panel in CSDP" 
   caption="Add Delivery Pipeline panel in CSDP"
   max-width="30%" 
   %}
{:start="3"}
1. Enter a name for the delivery pipeline.  
  The name is created from the names of the sensor and the trigger event for the delivery pipeline.   
  * **Sensor Name**: The name of the sensor resource, for example, `sensor-csdp-ci`.
  * **Trigger Name**: The event configured in the sensor to trigger pipeline execution, for example, `push-csdp-ci`.
1. Select the **Codefresh Starter Template**.  
1. From the list of **Git Sources**, select the Git Source to which to commit the resources for this delivery pipeline.  
  > Do not select the marketplace Git Source as you cannot commit to it.   
    If you have multiple runtimes installed, the Git Source you select also determines the runtime that executes the pipeline.
1. Select **Next**.  
  In the **Configuration** tab, **Workflow Templates** is selected. Our CI Starter Workflow Template is shown.   
  Copy and paste any predefined Argo Workflow template you want to work with, or edit the starter template as needed.  
  > Tip:  
  >  CSDP notifies you that you have uncommitted changes. The Commit button remains disabled until you define at least on Trigger Condition, as described in the next step.
  
  {% include 
   image.html 
   lightbox="true" 
   file="/images/pipeline/create/pipeline-wizard-workflow-tab.png" 
   url="/images/pipeline/create/pipeline-wizard-workflow-tab.png" 
   alt="Workflow Template tab in create delivery pipeline wizard" 
   caption="Workflow Template tab in create delivery pipeline wizard"
   max-width="30%" 
   %}

{:start="7"}
1. Select **Trigger Conditions**. 
  * From the **Add** dropdown, select **Git Events**.
  * In the **Git Repository URLs** field, select one or more repositories from the list to listen to for the selected event. 
     
      {% include 
      image.html 
      lightbox="true" 
      file="/images/pipeline/create/pipeline-wizard-trigger-git-repos.png" 
      url="/images/pipeline/create/pipeline-wizard-trigger-git-repos.png" 
     alt="Select Git repos to monitor for events" 
     caption="Select Git repos to monitor for events"
     max-width="30%" 
     %}

  * From the **Event** dropdown, select the event to trigger the pipeline. For example, **Commit pushed**.  
    CSDP displays all the **Arguments** available for the selected event.    
    Map each argument to a single or combination of predefined variables. In each field, type `$` and from the list of predefined variables, select the one you need.  
    CSDP automatically maps to the correct path when you commit the changes. Argo Workflow then instantiates the values from the event payload.  
    
    {% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/quick-start/quick-start-ci-pipeline-arguments.png" 
   url="/images/getting-started/quick-start/quick-start-ci-pipeline-arguments.png" 
   alt="Predefined variables for argument definitions" 
   caption="Predefined variables for argument definitionss"
   max-width="30%" 
   %}
{:start="8"}
1. To create dependencies for the event type from the event payload:
  * From **Event Filters**, select the filter, and select **+Add**.
  * Select the operational expression and define the value of the filter.    
  
  {% include 
   image.html 
   lightbox="true" 
   file="/images/pipeline/create/pipeline-wizard-event-filters.png" 
   url="/images/pipeline/create/pipeline-wizard-event-filters.png" 
   alt="Event dependencies with filters" 
   caption="Event dependencies with filters"
   max-width="30%" 
   %}
{:start="9"}
1. To create customized filters with values that you define, and not populated from the event payload, select **Detach & Customize**.

{:start="10"}
1. Select **Apply**, and then **Commit** on the top-right.
  The Commit Changes panel shows the resource files created for the pipeline. These files are created in the Git Source you selected in the first step of the wizard.
    {% include 
   image.html 
   lightbox="true" 
   file="/images/pipeline/create/pipeline-wizard-resources.png" 
   url="/images/pipeline/create/pipeline-wizard-resources.png" 
   alt="Pipeline resource files created on commit" 
   caption="Pipeline resource files created on commit"
   max-width="30%" 
   %}
{:start="11"}
1. Enter the commit message and then select **Commit**.
1. In the **Delivery Pipelines** page to which you are redirected, verify that your pipeline is displayed. 

 CSDP commits the pipeline to the Git repository, and then syncs it to the cluster. Wait for a few seconds for the sync to complete, and verify that the pipeline is displayed in the [Delivery Pipelines](https://g.codefresh.io/2.0/pipelines){:target="\_blank"} page.

