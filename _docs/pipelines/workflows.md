---
title: "Managing workflows"
description: ""
group: pipelines
toc: true
---


In Codefresh, workflows are submitted when pipelines are triggered. A workflow executes a series of tasks through one or more templates defined in its specifications. 
Argo defines the Workflow as the most important resource in Argo, defining the workflow to be executed, and storing the state of the workflow. For more information, see [Argo Workflows documentation](https://argoproj.github.io/argo-workflows/).   

In Codefresh, after submitting a workflow, you can:

* Track workflow performance and metadata  
  Track active workflows in the Workflows dashboard, or track workflows in the context of the pipelines that initiated them. 

  {% include image.html 
    lightbox="true" 
  file="/images/workflows/workflow-dashboard-overview.png" 
  url="/images/workflows/workflow-dashboard-overview.png"
  alt="Workflows dashboard"
  caption="Workflows dashboard"
  max-width="60%"
  %} 

* Manage workflow executions
  * Analyze and troubleshoot workflow steps: Analyze each step in the workflow, including steps related to Argo Events that triggered the pipeline.
  * View workflow logs: Real-time logs for ongoing workflows, and archived logs for completed workflows.
  * Manage workflows: Based on the current status of the workflow, suspend, stop, terminate ongoing workflows, and retry, resubmit, or delete completed workflows.

>This article focuses on workflow management. For information on managing Delivery Pipelines, see [Managing Delivery Pipelines]({{site.baseurl}}/docs/pipelines/managing-pipelines/).


### Workflow performance in dashboards



* Workflows dashboard: The Workflows dashboard displays a list of all the _active_ workflows across all Delivery Pipelines. To view a subset of workflows, defining filters, including pipelines to focus on specific workflows. From the Workflows dashboard, you can drill down directly into any workflow.

   {% include image.html 
  lightbox="true" 
  file="/images/pipeline/monitoring/monitor-workflows-tab.png" 
  url="/images/pipeline/monitoring/monitor-workflows-tab.png"
  alt="Workflow-level view in Delivery Pipeline"
  caption="Workflow-level view for Delivery Pipeline"
  max-width="30%"
  %}
  

* Workflows in Delivery Pipelines: The Delivery Pipelines page displays all the pipelines you created, including those without any workflows submitted. Selecting a pipeline displays all the workflows for the selected pipeline. When you view a workflow in the context of its pipeline, the pipeline's configuration settings, and the sensor and event-source manifests are available in the same location. This integrated view of both the workflow steps, workflow logs, and the pipeline configuration in the same location makes it easy to make the necessary changes when troubleshooting.  

  {% include image.html 
    lightbox="true" 
  file="/images/pipeline/monitoring/pipeline-list-view.png" 
  url="/images/pipeline/monitoring/pipeline-list-view.png"
  alt="Workflows in a Delivery Pipeline"
  caption="Workflows in a Delivery Pipeline"
  max-width="60%"
  %}


The table describes the main features shared by the Workflows dashboard and the Workflows tab in Delivery Pipelines.

{: .table .table-bordered .table-hover}
| Item                |  Description|  
|---------------------|-------------|
| **Filters**         | The filters available to customize the view, either in the Workflows dashboard, or for the selected pipeline. The filters are identical for both views. In the Workflow dashboard, selecting **More filters** allows you to filter workflows by one or more pipelines. For details, see [aggregated view]({{site.baseurl}}/docs/pipelines/managing-pipelines/#filters-for-aggregated-views). |                             
| **Workflow chart**         | The bar chart displaying the workflows for the selected time range. Up to 50 workflows are displayed, starting with the oldest. The color of the bar indicates if the workflow is active (blue), completed successfully (green), failed (red), or unknown (purple). To see a pop-up with workflow-specific information, mouse over the bar, and select **View Workflow Details** link. (see image below entitled, Pop-up with workflow metadata).  |          
| **Workflow entries**  | Every entry displays workflow-specific metadata and links, similar to what you see in the bar chart pop-ups. You can see the git hash, the name of the user who made the commit, the pipeline that submitted the workflow, the start time and duration. Selecting **View Workflow Details** drills down into the Workflows tab with execution details, with visualization of workflow steps for troubleshooting, analysis, and management.|  

  {% include image.html 
  lightbox="true" 
  file="/images/pipeline/monitoring/monitor-workflows-tab-popup.png" 
  url="/images/pipeline/monitoring/monitor-workflows-tab-popup.png"
  alt="Pop-up with workflow metadata in bar chart"
  caption="Pop-up with workflow metadata in bar chart"
  max-width="30%"
  %}

### Select a workflow 
Select the workflow you want to focus on, either from the Workflows dashboard or from a Delivery Pipeline. 

1. In the [Workflows dashboard](https://g.codefresh.io/2.0/workflows), set filters to view the subset you are interested in. 
  OR  
  In the **Delivery Pipelines** page, select a pipeline, and then select the [Workflows](https://g.codefresh.io/2.0/workflows) tab to see all its workflows. 

1. To drill down into a workflow, in the workflow entry, select **View workflow details**.

  {% include image.html 
    lightbox="true" 
  file="/images/workflows/workflow-drill-down.png" 
  url="/images/workflows/workflow-drill-down.png"
  alt="View Workflow Details"
  caption="View Workflow Details"
  max-width="50%"
  %}
  

### Analyzing workflow executions

Detailed information 
For running or completed workflows, review the steps as the workflow is being executed. Analyze failed steps with the help of logs and additional information. Change the 

View the connection between steps in the workflow, the status of each step, and additional information for the step. View subsets of steps in the workflow using filters.

* Visualize the entire flow, starting with the Argo Events that triggered the workflow, followed by the steps in the workflow itself. 

* View detailed information on a step in a convenient pull-out panel. Easily copy paths for attributes from event payloads, view logs for pods, and download artifacts.

{% include image.html 
  lightbox="true" 
  file="/images/workflows/workflow-steps.png" 
  url="/images/workflows/workflow-steps.png"
  alt="Workflow steps"
  caption="Workflow steps"
  max-width="30%"
  %}


##### Error notification for failed steps
The status bar alerts you to failed steps in workflows through an error notification with the number of errors. Select the notification to see the list of steps in the Errors tab.  

##### Workflow step types and filters

**Step status**  
Every step has a status indication of either active ({::nomarkdown}
<img src="../../../images/icons/active.png">{:/}), synced ({::nomarkdown}
<img src="../../../images/icons/success.png">{:/}), or failed ({::nomarkdown}
<img src="../../../images/icons/error.png">{:/}).

**Step types and filters**  
Steps in a workflow are identified as Kubernetes resources (Pods, Containers), Argo template types and template invocators (DAG, Step, StepGroup), or Argo Event resources (Event Sources, Sensors). 
https://argoproj.github.io/argo-workflows/workflow-concepts/#template-types
Event-type steps have labels within the step to differentiate them from workflow steps.

{% include image.html 
  lightbox="true" 
  file="/images/workflows/argo-events-in-wrkflow.png" 
  url="/images/workflows/argo-events-in-wrkflow.png"
  alt="Argo Event steps in workflow"
  caption="Argo Event steps in workflow"
  max-width="30%"
  %}

Filter by step type (Node), status (Phase), and Argo event resources (Resource).

 {% include image.html 
  lightbox="true" 
  file="/images/workflows/filters-workflow-steps.png" 
  url="/images/workflows/filters-workflow-steps.png"
  alt="Filters for workflow steps"
  caption="Filters for workflow steps"
  max-width="30%"
  %}


### Additional information for steps
To view additional information for a step, select the step.  

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

### View logs for workflows

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

### Manual actions for running/completed workflows
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
| **Delete**         |Delete an unused or legacy completed workflow. Deleting a workflow removes it from the pipeline's workflow list and from the Workflows dashboard.|         
| **Logs**           |[View logs for the workflow](#view-logs-forworkflows). | `                           

### View native workflows
Actions for active workflows
