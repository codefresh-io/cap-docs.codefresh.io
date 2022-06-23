---
title: "Managing workflows"
description: ""
group: pipelines
toc: true
---


Workflows are submitted when pipelines are triggered. A workflow executes a series of steps through one or more templates defined in its specification.  

Workflows in Codefresh are integrated with Argo Workflows, an open-source container-native workflow engine for orchestrating parallel jobs on Kubernetes, implemented as a K8s CRD (Custom Resource Definition). Argo defines the Workflow as the most important resource in Argo, defining both the workflow to be executed, and storing the state of the workflow. For more information, see [Argo Workflows documentation](https://argoproj.github.io/argo-workflows/).   


* Track ongoing and completed workflows
  View and monitor submitted workflows across all pipelines in the Workflows dashboard. Select a time range, or view up to fifty of the most recent workflows for all the pipelines in the runtime. Use filters to customize the dashboard view. Drill down to any workflow for further analysis.

   {% include image.html 
    lightbox="true" 
  file="/images/workflows/workflow-dashboard-overview.png" 
  url="/images/workflows/workflow-dashboard-overview.png"
  alt="Workflows dashboard"
  caption="Workflows dashboard"
  max-width="60%"
  %} 
  
* Track workflow performance by pipelines 
  Track workflow performance in the context of the pipelines from which they were submitted. 
  See key performance KPIs for the workflows such as the success rate, execution rate, and the KPIs for the steps across all workflows. 

  {% include image.html 
  lightbox="true" 
  file="/images/workflows/workflow-dashboard-overview.png" 
  url="/images/workflows/workflow-dashboard-overview.png"
  alt="Workflows dashboard"
  caption="Workflows dashboard"
  max-width="60%"
  %} 

* Analyze and troubleshoot workflow executions
  Select a workflow to take actions on it, in the context of the pipeline from which it was submitted, or  
  * Analyze each step in the workflow, including steps related to the events that triggered the pipeline.
  * Troubleshoot failed steps through real-time logs for ongoing workflows, and archived logs for completed workflows
  * Manage workflows, based on the current status; suspend, stop, terminate ongoing workflows, and retry, resubmit, or delete completed workflows.

>This article focuses on workflow management. For information on managing Delivery Pipelines, see [Managing Delivery Pipelines]({{site.baseurl}}/docs/pipelines/managing-pipelines/).


###  Tracking workflows in Workflows dashboard
The Workflows dashboard displays a list of all the _active_ workflows across all Delivery Pipelines. Active workflows are either running or completed workflows. Set filters to view subsets of workflows, including pipeline filters to focus on specific workflows. Then drill down on any workflow to view execution details, and manage it.

   {% include image.html 
  lightbox="true" 
  file="/images/pipeline/monitoring/monitor-workflows-tab.png" 
  url="/images/pipeline/monitoring/monitor-workflows-tab.png"
  alt="Workflow-level view within Delivery Pipeline"
  caption="Workflow-level view within Delivery Pipeline"
  max-width="30%"
  %}

The table describes the main features in the Workflows dashboard.

{: .table .table-bordered .table-hover}
| Item                |  Description|  
|---------------------|-------------|
| **Filters**         | The filters available to customize the view. Selecting **More filters** allows you to filter workflows by one or more pipelines. For details, see [aggregated view]({{site.baseurl}}/docs/pipelines/managing-pipelines/#filters-for-aggregated-views). |                             
| **Workflow chart**         | The bar chart displaying the workflows for the selected time range. Up to 50 workflows are displayed, starting with the oldest. The color of the bar indicates if the workflow is active (blue), completed successfully (green), failed (red), or unknown (purple). For workflow-specific metadata, mouse over the bar. To drill down on that workflow, select **View Workflow Details** link. (see image below entitled, Pop-up with workflow metadata).  |          
| **Workflow list**  | The list of workflows with workflow-specific metadata and links, similar to what you see in the bar-chart pop-ups. You can see the git hash, the name of the user who made the commit, the pipeline that submitted the workflow, the start time and duration. Selecting **View Workflow Details** opens the Workflows tab with execution details,  visualization of workflow steps for troubleshooting, analysis, and management.|  

  {% include image.html 
  lightbox="true" 
  file="/images/pipeline/monitoring/monitor-workflows-tab-popup.png" 
  url="/images/pipeline/monitoring/monitor-workflows-tab-popup.png"
  alt="Pop-up with workflow metadata in bar chart"
  caption="Pop-up with workflow metadata in bar chart"
  max-width="30%"
  %}

### Tracking workflows by pipeline
The Delivery Pipelines dashboard shows insights into the collective performance of the workflows in the selected pipeline. Performance KPIs are available at the workflow-level and at the step-levels.  See success and execution rates for the workflows, and average durations and resource consumption for workflow steps. Compare with the performance in the reference period to derive insights. 
 
1. From the **Delivery Pipelines** page, select a pipeline to see its workflows.
1. Select the [Dashboard](https://g.codefresh.io/2.0/pipelines/edit/codefresh-v2-production/codefresh-v2-production/argo-platform-push%2Fservice-yaml/dashboard){:target="\_blank"} tab. 

  {% include image.html 
  lightbox="true" 
  file="/images/workflows/performance-kpis.png" 
  url="/images/workflows/performance-kpis.png"
  alt="Workflow performance for selected pipeline"
  caption="Workflow performance for selected pipeline"
  max-width="60%"
  %}

**Aggregated workflow analytics**  

The upper half shows KPIs for all workflows submitted for the pipeline. To customize the view, select filters. In this view, you can also filter by **Branch** which is the Git branch or branches with the events that triggered the pipeline.   

**Step-level analytics**  
The Step Analytics list view in the lower half shows the KPI breakdown per step, and additional information on the step. Step analytics show the aggregated average for KPIs in each step of a workflow, across all active workflows for the selected pipeline.  
Analyze performance through the KPI metrics to identify how the metrics are trending compared to the reference period 
> Each metric also shows the difference in percentage compared to the reference period corresponding to the Time range selected.  


Here's an example of step information for all workflows in the Dashboard tab.

   {% include image.html 
  lightbox="true" 
  file="/images/pipeline/monitoring/pipeline-list-view.png" 
  url="/images/pipeline/monitoring/pipeline-list-view.png"
  alt="Step-level analytics for workflows in a Delivery Pipeline"
  caption="Step-level analytics for workflows in a Delivery Pipeline"
  max-width="30%"
  %}


{: .table .table-bordered .table-hover}
| Step Analytics        |  Description|  
| --------------| --------------           |  
| **Step**      | The name of the step in the workflow. |                             
| **Type**      | The action executed by the step. For example, the step can execute a Container template, or skip the step. For details, see [Template types in Core Concepts](https://argoproj.github.io/argo-workflows/workflow-concepts/#template-types){:target="\_blank"}.|          
| **Template**  | The template called by the step.  |       
| **Workflow Template**| The Workflow Template referenced by the step. Different steps can reference the same or different Workflow Templates.| 
| **Avg. Duration**| The average length of time for all active workflows to complete execution of this step, in mm:ss. |   
| **Executions**| The number of times the step was executed. |  
| **CPU** and **Memory**| The average CPU and memory consumed by the step, across all the active workflows.|  
| **Errors**| The number of times that the execution of this step resulted in errors.|  


### Managing workflows

Both the Workflow and the Delivery Pipeline dashboards allow you to drill down into a specific workflow to view, analyze, and troubleshoot execution.  When you drill down from a Delivery Pipeline, you can see the workflow in the context of its pipeline, including the pipeline's configuration settings, and sensor and event-source manifests. The integrated view of both the workflow steps, workflow logs, and the pipeline configuration in the same location makes it easy to make changes when troubleshooting.  

  {% include image.html 
    lightbox="true" 
  file="/images/pipeline/monitoring/pipeline-list-view.png" 
  url="/images/pipeline/monitoring/pipeline-list-view.png"
  alt="Workflows in a Delivery Pipeline"
  caption="Workflows in a Delivery Pipeline"
  max-width="60%"
  %}


#### Select a workflow 
Select the workflow you want to focus on, either from the Workflows dashboard or from a Delivery Pipeline. 

1. In the [Workflows dashboard](https://g.codefresh.io/2.0/workflows), set filters to view the subset you are interested in. 
  OR  
  In the **Delivery Pipelines** page, select a pipeline, and then select the [Workflows](https://g.codefresh.io/2.0/workflows) tab to see all its workflows. 

1. To drill down into a workflow, select **View workflow details** for that workflow.

  {% include image.html 
    lightbox="true" 
  file="/images/workflows/workflow-drill-down.png" 
  url="/images/workflows/workflow-drill-down.png"
  alt="View Workflow Details"
  caption="View Workflow Details"
  max-width="50%"
  %}
  

#### Visualizing workflow steps

View the connection between steps in the workflow, the status of each step, and additional information for the step. 

* Visualize the entire flow, starting with the Argo Events that triggered the workflow, followed by the steps in the workflow itself. 
* Isolate failed steps to troubleshoot them
* View detailed information on a step in a pull-out panel. Easily copy paths for attributes from event payloads, view logs for pods, and download artifacts.

{% include image.html 
  lightbox="true" 
  file="/images/workflows/workflow-steps.png" 
  url="/images/workflows/workflow-steps.png"
  alt="Workflow steps"
  caption="Workflow steps"
  max-width="30%"
  %}

#### Workflow steps - status, types, and filters

**Step status**  
Every step has a status indication of either active ({::nomarkdown}
<img src="../../../images/icons/active.png">{:/}), synced ({::nomarkdown}
<img src="../../../images/icons/success.png">{:/}), or failed ({::nomarkdown}
<img src="../../../images/icons/error.png">{:/}).

**Step types and filters**  
Steps in a workflow are either Kubernetes resources (Pods, Containers for example), Argo templates and template invocators (DAG, Step, StepGroup for example), or Resources (Event Sources, Sensors). For more information, see [Template types in Core concepts](https://argoproj.github.io/argo-workflows/workflow-concepts/#template-types).


> Important: Node and Phase filters for steps have an _AND_ relationship. To see results, you must select at least one Node type and Phase status.


 {% include image.html 
  lightbox="true" 
  file="/images/workflows/filters-workflow-steps.png" 
  url="/images/workflows/filters-workflow-steps.png"
  alt="Filters for workflow steps"
  caption="Filters for workflow steps"
  max-width="30%"
  %}



Resource-type steps have labels within the step to differentiate them from workflow steps.

{% include image.html 
  lightbox="true" 
  file="/images/workflows/argo-events-in-wrkflow.png" 
  url="/images/workflows/argo-events-in-wrkflow.png"
  alt="Argo Event steps in workflow"
  caption="Argo Event steps in workflow"
  max-width="30%"
  %}


### Troubleshooting failed steps
Identify failed steps in a workflow through the error notifications alert in the workflow status bar, through Phase filters, or the search mechanism. With Phase status filters, remember to also select at least one Node type for results.  
For further analysis, select the step, or click Logs in the toolbar.  


**Error notifications for failed steps**  

* The status bar alerts you to failed steps through an error notification.   
  Clicking the notification opens the list of steps in the Errors tab.  

{% include image.html 
  lightbox="true" 
  file="/images/workflows/workflow-error-notification.png" 
  url="/images/workflows/workflow-error-notification.png"
  alt="Error notifications for failed steps"
  caption="Error notifications for failed steps"
  max-width="30%"
  %}

 {% include image.html 
  lightbox="true" 
  file="/images/workflows/workflow-error-notifications-open.png" 
  url="/images/workflows/workflow-error-notifications-open.png"
  alt="Error notifications panel"
  caption="Error notifications panel"
  max-width="30%"
  %}

  In the Errors tab,  are links to the logs.

 {% include image.html 
  lightbox="true" 
  file="/images/workflows/workflow-error-notifications-open.png" 
  url="/images/workflows/workflow-error-notifications-open.png"
  alt="Error notifications panel"
  caption="Error notifications panel"
  max-width="30%"
  %}

**Error and Failed Phase filters**  

Selecting the Error and Failed status filters isolates the steps in the workflow. 


 {% include image.html 
  lightbox="true" 
  file="/images/workflows/workflow-steps-failed.png" 
  url="/images/workflows/workflow-steps-failed.png"
  alt="Identifying failed steps with Phase filters"
  caption="Identifying failed steps with Phase filters"
  max-width="30%"
  %}

**Search for failed steps**  
If you know the name of the steps, use the Search field to navigate to failed steps.

{% include image.html 
  lightbox="true" 
  file="/images/workflows/failed-steps-search.png" 
  url="/images/workflows/failed-steps-search.png"
  alt="Search for failed steps"
  caption="Search for failed steps"
  max-width="30%"
  %}



### Detailed information for steps
To get detailed information for a step, select the step.  

The tabs displayed differ according to the step type:  

  * Almost all workflow step types show the Summary, Manifest, Containers, Inputs, Outputs. 
  * Pod step-types also display the Logs tab.
  * Event-step types show Manifest, Summary and Payload.
    > For Cron and Unknown event types, only the Event Sources are shown. 
  
  Example: event-source manifest.

  {% include image.html 
  lightbox="true" 
  file="/images/workflows/event-source.png" 
  url="/images/workflows/event-source.png"
  alt="Additional info for event-source manifest in workflow"
  caption="Additional info for event-source manifest in workflow"
  max-width="30%"
  %}

  Example: event-payload.

 {% include image.html 
  lightbox="true" 
  file="/images/workflows/event-payload.png" 
  url="/images/workflows/event-payload.png"
  alt="Additional info for event payload in workflow"
  caption="Additional info for event payload in workflow"
  max-width="30%"
  %}

 Example: pod step type

 {% include image.html 
  lightbox="true" 
  file="/images/workflows/workflow-pod-step.png" 
  url="/images/workflows/workflow-pod-step.png"
  alt="Additional info for pod step type in workflow"
  caption="Additional info for od step type in workflow"
  max-width="30%"
  %}

### View logs for steps or workflows

View logs for ongoing or completed workflows. As with logs in any standard terminal, you can copy/cut/paste log info. The commands differ based on the OS.

* For ongoing workflows, you can see live logs as the steps in the workflow are being executed.  
* For completed workflows, you can view logs for any step in the workflow.  

> To view logs for completed workflows, you must configure an artifact repository in CSDP to archive them. 

**Before you begin**  
[Configure an artifact repository]({{site.baseurl}}/docs/pipelines/configure-artifact-repository/)  

**How to**  
1. Select the [workflow](#select-the-workflow-to-monitor). 
1. Filter by **Node Type** of **Pods**, and then select a step to view its logs.

 {% include image.html 
  lightbox="true" 
  file="/images/workflows/view-logs-pull-out.png" 
  url="/images/workflows/view-logs-pull-out.png"
  alt="Event payload in workflow"
  caption="Event source in workflow"
  max-width="30%"
  %}


OR

1. In the Workflows tab, select **Logs**.
1. From the **Select Step (Pod)** dropdown, do one of the following:
  * To view live logs for an ongoing workflow, select **All**.
  * To view archived logs for a completed workflow, select the step and from the **Select Container** dropdown, select the container. 

  {% include image.html 
  lightbox="true" 
  file="/images/workflows/view-logs-logs-tab.png" 
  url="/images/workflows/view-logs-logs-tab.png"
  alt="Event payload in workflow"
  caption="Event source in workflow"
  max-width="30%"
  %}

{:start="3"}
1. If needed, copy/cut/paste log details. Refer to the table below for help on commands compatible with your OS.


{: .table .table-bordered .table-hover}
|OS/terminal emulator|Cut             |Copy           |Paste    | 
| --------------     | -----          | -----------   | --------| 
| **Apple**          | `Command`+`X`                      | `Command`+`C`                      | `Command`+`V`                    |                            
| **Windows/ GNOME/KDE**| `Control`+`X` or `Shift`+`Delete`| `Control`+`C` or `Control`+`Insert`| `Control`+`V` or `Shift`+`Insert`|   
| **GNOME/KDE terminal emulators**|N/A|`Control`+`Shift`+`C` or `Control`+`Insert`| `Control`+`Shift`+`V` or `Control`+`Shift`+`Insert`; (to paste selected text `Shift`+`Insert` or middle mouse button) |         

### Actions for running/completed workflows
When you drill down into the workflow details for a running or completed workflow, the toolbar displays several actions.   
>The actions in the toolbar are _manual_ actions, different from the same actions that are built into the template which are executed automatically. For example, the Suspend option in the toolbar can be configured in the template as well. 

{: .table .table-bordered .table-hover}
|Action                   |Description                                  | 
| --------------          | ----------------------------------          | 
| **Summary**             | Detailed information for both active and completed workflows.  | 
| **Suspend**/**Resume**  | Temporarily pause execution of a running workflow. Resume execution from the point at which it was suspended. | 
| **Stop**                | Stop execution of an active workflow through a graceful shutdown. Graceful shutdown implements the actions configured in the ExitHandler, such as cleaning up resources, sending Slack notifications, for example. Compare with Terminate.|                         
| **Retry**             | Restart a completed but failed workflow. You may want to retry a failed workflow, to retry end-to-end testing steps for example.  The retry option is available only for workflows with errors.                      | `                           
| **Resubmit**       | Resubmit a successfully completed workflow to create a new instance of the workflow. |   
| **Terminate**      | Force shutdown to stop the execution of an active workflow. Force shutdown summarily terminates the workflow ignoring the ExitHandler.  | `                           
| **Delete**         |Delete an unused or legacy completed workflow. Deleting a workflow removes it from the pipeline's  list and from the Workflows dashboard.|         
| **Logs**           |[View logs for the workflow](#view-logs-forworkflows). | `                           

### View native workflows
View the workflow in the Argo UI. 


