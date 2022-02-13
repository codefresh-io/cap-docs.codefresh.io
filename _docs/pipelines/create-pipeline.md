

Delivery Pipelines are where all the magic happens in CSDP. Delivery pipelines connect all the dots in the CI/CD lifecycle. The pipelines are optimized for DevOps flows for checking out code, building and testing artifacts. They also work well for general purposes like data pipelines.  

CSDP builds on and integrates with Argo Workflows and Argo Events, and we do with our Delivery Pipeline wizard. They bring the power of Argo Workflows and Argo Events, while greatly simplifying the creation and monitoring of workflows along with their triggers. 


### Delivery pipeline concepts
Let's start by reviewing main concepts of CSDP delivery pipelines - how we integrate with Argo Workflows and Argo Events, and CSDP's unique functionality and enhancements.  

#### Pipeline per event type
The name of the delivery pipeline is created from the name of the sensor and the name of the trigger, which is the event type that triggers the pipeline.
A sensor-trigger-type pair is a unique pipeline in CSDP. The same sensor can be associated with different trigger types.

Each delivery pipeline links through a Git Source to a Git repository that houses the resources of the delivery pipeline. All commits to this pipeline's resources are made to the repo. The Git Source also determines which CSDP runtime executes the pipeline.



#### Argo Workflows
CSDP is fully integrated with Argo Workflows, and implements Argo Workflows as a Kubernetes CRD (Custom Resource Definition).  
Delivery pipelines start with the Workflow Template that implements the CI/CD 
 
If you are looking for Workflow Template resources, both Argo and Codefresh have examples and libraries of Workflow Templates you can use.
* For conceptual information on Argo Workflows, read the [official documentation](https://argoproj.github.io/argo-workflows/){:target="\_blank"}.
* For examples of Workflow Templates in Argo, see their [documentation by example](https://github.com/argoproj/argo-workflows/blob/master/examples/README.md){:target="\_blank"} page.
* And of course, Codefresh offers a fully-certified and always expanding library of ready-to-use Workflow Templates in [Codefresh Hub for Argo](https://codefresh.io/argohub/){:target="\_blank"}.

In the pipeline wizard, we have our starter Workflow Template to use as a base, or copy and paste a predefined Workflow Template, and then modify as needed. 
> If your Workflow Template, generates artifacts, you must [configure an artifact repository in CSDP]({{site.baseurl}}/docs/pipelines/configure-artifact-repository).

#### Globally-scoped template arguments
CSDP isolates all input parameters in Workflow Templates for visibility, ease of use, and tracking.  
The **Arguments** tab in the delivery pipeline creation wizard exposes these globally-scoped input parameters in the Workflow Template. You can set default values here.
{% include 
   image.html 
   lightbox="true" 
   file="/images/pipeline/create/pipeline-wizard-arguments-tab.png" 
   url="/images/pipeline/create/pipeline-wizard-arguments-tab.png" 
   alt="Arguments tab in delivery pipeline wizard" 
   caption="Arguments tab in delivery pipeline wizard"
   max-width="30%" 
   %} 


Another unique feature is that all through pipeline executions and workflow deployments, CSDP not ony updates the argument list with changes, but also records the changes, such as addition or removal of arguments.  

You have a single point of reference, and complete visibility into the type of change. New arguments are tagged as `New`. Deleted arguments are tagged as `Not consumed`.
{% include 
   image.html 
   lightbox="true" 
   file="/images/pipeline/create/pipeline-arguments-modified.png" 
   url="/images/pipeline/create/pipeline-arguments-modified.png" 
   alt="Arguments tab showing modifications" 
   caption="Arguments tab showing modifications"
   max-width="30%" 
   %} 

> Important:  
>  If the same argument is used by event type, the value defined for it in the Trigger Conditions take precedence over the template value.

QUESTIONS: If i define a value, is it carried over to Trigger Conditions?

#### Argo Events
As with Argo Workflows, CSDP supports Argo Events. In CSDP, there is no need to manually create and define sensor dependencies for Argo Events. The wizard guides you through event source and event type selection, and trigger configurations.  

For conceptual information on Argo Events, read the [official documentation](https://argoproj.github.io/argo-events/){:target="\_blank"}.


In the delivery wizard, **Trigger Conditions** collates all the requirements to set up and configure Argo Events and their dependencies. Here, you select the event source, the event type, arguments and filters for the event type, all in one location. On commit, CSDP generates the manifests for Workflow Template, event source, event bus, and the sensor.

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
You can select the code repos for event triggers. The code repos you select are different from the Git Source-linked repo where you commit pipeline resources.

**Event sources and Event Types**
Currently we support GitHub as an Event Source. Every event source has a set predefined event types you can select from. 

**Arguments**  
Every event type supports a set of arguments you can configure for that event. Some arguments are referenced in the Workflow Template, and some are unique to the event type. As with Workflow Template-level arguments, you can add default values, or create argument definitions with variables for dynamic updates.  

Every argument has a set of predefined variables you can select from. CSDP automatically maps the argument to the path in the manifest generated on commit. You don't need to remember or be aware of resource paths for event-type arguments as CSDP does it for you. Argo Workflows extracts the value from the event payload at runtime. 

> If the event type argument is also a Workflow Template input argument, the value defined in Trigger Conditions take precedence. 

**Filters**  
Filters create dependencies for event type triggers. Use filters to enforce validity constraints on what triggers an event. For example, add a filter to a Git push event that triggers that event only on push to a particular _branch_.   

CSDP supports Argo Events Data filters.

QUESTION: what is the usability in Detach filters?



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

### Step-by-step
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
1. Select **Trigger Conditions**. 
  * From the **Add** dropdown, select **Git Events**.
  * In the **Git Repository URLs** field, type the URL to the Git repo, or select one or more repositories from the list to listen to for the selected event. 
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
   alt="Predefined variables for arguments" 
   caption="Predefined variables for arguments"
   max-width="30%" 
   %}
{:start="8"}
1. To create dependencies for the event type from the event payload:
  * From **Event Filters** dropdown, select the filter and select **+Add**.
  * Select the expression and define the value of the filter.    
  
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
To create customized filters"

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

 CSDP commits the pipeline to the Git repository, and then syncs it to the cluster. Wait for a few seconds until the sync is complete, and verify that the pipeline is displayed in the [Delivery Pipelines](https://g.codefresh.io/2.0/pipelines){:target="\_blank"} page.

