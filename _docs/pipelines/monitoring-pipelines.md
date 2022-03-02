---
title: "Pipeline monitoring"
description: ""
group: pipelines
toc: true
---

Once a pipeline runs, a workflow is submitted, CSDP continuoyly collects and displays data on the pipeline, the workflows. 
This article describes how youn to monitor pipeline and workflow states 

Activity Log notifications: Monitor logs for sensor and event-sources. 
Aggregated and individual pipeline views: See data for pipelines and workflows, including metrics, at global or pipel-specific levels
Workflow visualizations in YAML and tree formats
Visualize data at different levels: from aggregated data for all active pipelines and their workflows, to a single pipeline and its workflows, and down to the steps in a single pipeline's workflows.
KPI metrics: Analyze resource consumption, and identify trends by comparing how metrics 





### How to: Monitor sensor and event source notifications in Activity Log
The Activity Log is a quick way to monitor sensor and event-source logs. A pull-down panel in the CSDP toolbar, the Activity Log shows ongoing, success, and error notifications, by date, starting with today's date. Syntax errors that prevent the sensor from triggering the pipeline are displayed here.

1. In the CSDP UI, on the top-right of the toolbar, select ![](/images/pipeline/monitoring/pipeline-activity-log-toolbar.png?display=inline-block) **Activity Log**.
1. To see pipeline activity, select **Event Source** and **Sensor** filters.

  {% include image.html 
  lightbox="true" 
  file="/images/pipeline/monitoring/monitor-activity-log.png" 
  url="/images/pipeline/monitoring/monitor-activity-log.png"
  alt="Activity Log filtered by Event Source and Sensor"
  caption="Activity Log filtered by Event Source and Sensor"
  max-width="30%"
  %}

{:start="3"}
1. To see more information on an error, select the **+** sign.

### Working with aggregated pipeline views
The aggregated view is a global view of all the pipelines with active workflows in your cluster. 


#### How to: Switch to aggregated pipeline view
1. In the CSDP UI, go to the [Home](https://g.codefresh.io/2.0/) page.
1. To customize the view, select filters.
1. To view day-to-day data for a metric, select a mini-chart. 


#### Filters for aggregated views
Filters allow you to narrow the pipeline data, and  customize the view with just the information you want to see. Unless otherwise indicated, every filter supports multi-selection.

{: .table .table-bordered .table-hover}
|  Filter          |  Description|  
| --------------   | --------------|  
| **Status**           | Succeeded: Pipelines with workflows completed successfully. **Failed**: Pipelines with workflows that failed. **Error**: Pipelines with workflows that resulted in errors. |                            
| **Repo**             |The Git repository or repositories with the events that triggered or ran the pipelines. |          
| **Event Type**       | The Git or Calendar event or events by which to view pipelines.  If you select Git push, only those pipelines configured to be run on Git push are displayed. |    
| **Initiator**       | The user who made the commit that triggered the event and ran the pipeline.|       
| **Time**             |The time frame for which to show pipeline data, and can be **Last 7 days** (the default), **Last 30 days**, or **Last 90 days**|

#### Metrics in aggregated views

Pipeline KPIs are available as line charts and list formats. 
* The line charts visualize the detailed day-to-day values for the metric in the selected time range.
* The list formats display the average values for the metric, and also shows the fluctuations in the values compared to a reference time range. The reference time range is the period of time that corresponds to and precedes the selected time range.

|  Metric               |  Description|  
| --------------        | --------------|  
| **Success Rate**      | The average number of successful executions, in percentage. |                            
| **Average Duration**  | The average length of time to complete execution, in mm:ss. |          
| **Executions**        | The average number of times the pipeline was triggered, in percentage. |    
| **Committers**        | The number of users whose commits on the repository or repositories triggered the pipelines. The user count is aggregated per user, so if the same user made ten commits, all commits are counted only once.|       


#### Understand aggregated pipeline view

Here is an example of the aggregated pipeline view. 

  {% include image.html 
  lightbox="true" 
  file="/images/pipeline/monitoring/monitor-aggregated-view.png" 
  url="/images/pipeline/monitoring/monitor-aggregated-view.png"
  alt="Aggregated view for Delivery Pipelines in Home page"
  caption="Aggregated view for Delivery Pipelines in Home page"
  max-width="30%"
  %}

The table describes the information in the aggregated pipeline view.


{: .table .table-bordered .table-hover}
| Legend        |  Description|  
| --------------| --------------           |  
| 1              | The Home page where aggregated views for Delivery Pipelines are displayed.|                             
| 2              | The filters by which to customize the aggregated view. For information on the filters, see [Filters quick reference] earlier in this article. All filters, except the **Time** filter support multi-selection.|          
| 3              | Micro line graph views of KPIs for Delivery Pipelines. Selecting a micro line chart displays the detailed day-to-day values for the metrics. For information on each metric, see [Metrics quick reference] earlier in this article. |       
| 4             | Pipelines by activity and duration. List views of up to ten of the most active, and ten of the longest running pipelines. <br>
{::nomarkdown} <ul> <li>Pipelines without numbers prefixing the names indicate new pipelines.  </li><li>Pipeline names prefixed with a green or red number (encircled in the image above) indicate existing pipelines. <br>
The number indicates the change in position of the pipeline compared to the reference period.  </li>
<li>To drill down into a specific pipeline, select the pipeline.</li></ul> {:/}|      

### Working with individual pipeline views

Individual pipeline view shows all pipelines in your cluster, including those that are not run and without workflows. 
When you select an individual pipeline, you can viiew and montior:
Steps within workflows
Workflows 

Both step and workflow analytics share the same set of filters that allow you to reduce the data displayed and customize the view.


#### How to: Switch to pipeline list view

1. In the CSDP UI, go to the [Delivery Pipelines](https://g.codefresh.io/2.0/pipelines) page.
1. To find specific pipelines, in the search field, enter a part of the pipeline name. For example, to find all CI-based pipelines, enter `ci`.

#### Understanding pipeline list view

Here is an example of the list view of individual pipelines. 

  {% include image.html 
  lightbox="true" 
  file="/images/pipeline/monitoring/pipeline-list-view.png" 
  url="/images/pipeline/monitoring/pipeline-list-view.png"
  alt="List view with individual Delivery Pipelines"
  caption="List view with individual Delivery Pipelines"
  max-width="30%"
  %}

The table describes the information for each pipeline in the list view.
> The list view displays data for the last seven days.


{: .table .table-bordered .table-hover}
| Legend        |  Description|  
| --------------| --------------           |  
| **Name**      | The name of the pipeline. Select the name to drill down into workflow steps and workflows.|                             
| **Recent Activity**| Bar chart of up to ten of the most recent workflows submitted for the pipeline. {::nomarkdown}<ul><li>Each bar represents a workflow, and is color-coded to indicate the workflow's status. Green is for completed, red for failed, and blue for ongoing workflows. </li><li> Mouse over a bar to see a pop-up with the metadata details for the workflow. </li> <li>Selecting View Workflow Details takes you to the Workflow tab where you have several options to manage the workflow  and view logs. For more information, see  XREF TO TOPIC.</li></ul> {:/}|          
| **Last Run**| The time of the most recent run of the pipeline (workflow). The time is also color-coded to indicate the status of the workflow.  |       
| **Success Rate**| The number of workflows that completed successfully in the last seven days, and the comparison to the reference period in percentage. | 
| **Avg. Duration**| The average length of time for the pipeline's workflows to complete execution, in mm:ss. |   
| **Weekly Runs**| The number of times the pipeline was run and its workflows submitted in the last seven days, and the difference in percentage compared to the reference period. |  
| **Runtime**| The runtime on which the pipeline is executed. This is the Git Source selected when you created the pipeline.  |  
| **Last Modified**| The date and time when the pipeline was recently updated. The updates include changes in configuration and manifests.|  
| **Sync Status**| The sync status with Git.|  



### Monitoring step analytics for workflows in pipeline 
Step analytics show the aggregated average KPIs for each step in a workflow, across all active workflows for the selected pipeline. 
Analyze performance through the KPI metrics and identify how the metric is trending, compared to the reference period.

**How to: View step analytics for workflows**  
1. Select a pipeline from either the **Home** page or the **Delivery Pipelines** page.
1. Select the [**Dashboard**](https://g.codefresh.io/2.0/pipelines/edit/codefresh-v2-production/codefresh-v2-production/argo-platform-push%2Fservice-yaml/dashboard) tab. 

**Step Analytics list information**
Here's an example of the Dashboard tab for a pipeline with step information.

   {% include image.html 
  lightbox="true" 
  file="/images/pipeline/monitoring/pipeline-list-view.png" 
  url="/images/pipeline/monitoring/pipeline-list-view.png"
  alt="Step-level analytics for workflows in Delivery Pipeline"
  caption="Step-level analytics for workflows in Delivery Pipeline"
  max-width="30%"
  %}


The upper half shows KPIs for all workflows submitted for the pipeline. To customize the view, select filters. All the filters are identical to those available for the aggregated view. In this view, you can also filter by **Branch** which is the Git branch or branches with the events that triggered the pipeline.  

The Step Analytics list view in the lower half shows the KPI breakdown per step and additional information on the step. 
> Each metric shows also the difference in percentage compared to the reference period corresponding to the Time range selected.  

{: .table .table-bordered .table-hover}
| Step Analytics        |  Description|  
| --------------| --------------           |  
| **Step**      | The name of the step in the workflow. |                             
| **Type**      | The Argo Workflow template type or action executed by the template. For details, see [Template types in Core Concepts](https://argoproj.github.io/argo-workflows/workflow-concepts/#template-types).|          
| **Template**  | The template executed by the step.  |       
| **Workflow Template**| The Workflow Template referenced by the step. Different steps can reference the same or different Workflow Templates.| 
| **Avg. Duration**| The average length of time for the workflows to complete execution, in mm:ss. |   
| **Executions**| The number of times the step was executed. |  
| **CPU** and **Memory**| The average CPU and memory consumed by the step, across all the active workflows.|  
| **Errors**| The number of times step execution resulted in errors.|  

### Monitoring workflows in pipelines
The Workflows tab shows information within the context of the selected pipeline. It shows all the workflows submitted for that pipeline, sorted from the most recent to the oldest.   

**How to: Monitor workflows for a pipeline**  
1. Select a pipeline from either the **Home** page or the **Delivery Pipelines** page.
1. Select the [**Workflows**](https://g.codefresh.io/2.0/pipelines/edit/codefresh-v2-production/codefresh-v2-production/argo-platform-push%2Fservice-yaml/workflows) tab. 

**Workflow list view**
Here's an example of the list of workflows for a pipeline, in the Workflow tab.

   {% include image.html 
  lightbox="true" 
  file="/images/pipeline/monitoring/monitor-workflows-tab.png" 
  url="/images/pipeline/monitoring/monitor-workflows-tab.png"
  alt="Workflow-level view in Delivery Pipeline"
  caption="Workflow-level view for Delivery Pipeline"
  max-width="30%"
  %}

The table describes the information in the list view.

| Legend        |  Description|  
| --------------| -------------|  
| **1**         | The filters to customize the workflow view for the pipeline. All the filters are identical to those available for the aggregated view. In this view, in addition to the predefined time ranges, you can select a custom time range.  |                             
| **2**         | The bar chart with the duration and status of all workflows submitted for the pipeline, starting with the most recent. The color of the bar indicates if the workflow it represents is in progress (blue), completed successfully (green), or failed with error (red). Mouse over the bar to see a pop-up of workflow-specific information (see image below entitled, Pop-up with workflow metadata).  |          
| **3**  | Workflow-specific information, similar to what you see in the bar chart pop-ups. You can see the git hash, the name of the user who made the commit, the pipeline that submitted the workflow, the start time and duration.|       
| **4**| Selecting **View Workflow Details** shows options to monitor and manage workflows, including viewing workflow logs.| 


   {% include image.html 
  lightbox="true" 
  file="/images/pipeline/monitoring/monitor-workflows-tab-popup.png" 
  url="/images/pipeline/monitoring/monitor-workflows-tab-popup.png"
  alt="Pop-up with workflow metadata"
  caption="Pop-up with workflow metadata"
  max-width="30%"
  %}







