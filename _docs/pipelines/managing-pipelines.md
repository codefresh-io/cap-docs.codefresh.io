---
title: "Managing pipelines"
description: ""
group: pipelines
toc: true
---

Once a pipeline is created, and a workflow submitted when it is triggered, Codefresh continuously collects and displays real-time analytics on the pipeline and its workflows.  
View global pipeline analytics through the Home page dashboard Monitor, analyze, and compare resource consumption for all pipelines or for a single pipeline
 
The Delivery Pipelines page is a list view of all pipelines that you have created, including those without workflows. Frill down into a pipeline to see its workflows, and view workflow details to troubleshoot and manage individual workflows.

There are several ways to monitor and manage pipelines and its workflows:

* Monitor sensor and event-source errors in the Activity Log
* Analyze global analytics for pipelines with active workflows
* Update pipeline configuration, such as the Workflow Template, Trigger conditions and arguments 
* Update event source and sensor manifests
* Troubleshoot workflows through workflow-step visualizations, alongside manifest, summary, and log details 
* Retry, resubmit, or terminate workflows
For details on workflows in pipelines, see 

The example below shows the list view for pipelines.
{% include image.html 
  lightbox="true" 
  file="/images/pipeline/monitoring/pipeline-list-view.png" 
  url="/images/pipeline/monitoring/pipeline-list-view.png"
  alt="List view with individual Delivery Pipelines"
  caption="List view with individual Delivery Pipelines"
  max-width="30%"
  %}


### Monitor sensor and event source notifications for pipelines in Activity Log
Monitor logs for sensors and event-sources in pipelines in the Activity Log. A pull-down panel in the Codefresh toolbar, the Activity Log shows ongoing, success, and error notifications, by date, starting with today's date. Syntax errors that prevent the sensor from triggering the pipeline are displayed here.

1. In the Codefresh UI, on the top-right of the toolbar, select ![](/images/pipeline/monitoring/pipeline-activity-log-toolbar.png?display=inline-block) **Activity Log**.
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

### Aggregated global analytics for pipelines
The Home page displays the aggregated, global view of all the Delivery Pipelines with active workflows in your cluster.  The global view provides insights into trends, and resource consumption across all the _active_ pipelines. ipelines by activity and by duration, and compare metrics Pipelines are Analytics are derived by comparing the selected time range to the corresponding reference period. If your time range is the last seven days, the reference period is the seven days that precede the time range. 


#### View aggregated pipeline analytics (global view)

1. In the Codefresh UI, go to the [Home](https://g.codefresh.io/2.0/){:target="\_blank"} page.
1. Set the **Time** frame for which to show pipeline data, as the **Last 7 days** (the default), **Last 30 days**, or **Last 90 days**.
1. Scroll down to Delivery Pipelines:
  1. To customize the view, select **Filters**.
  2. To switch to individual pipeline view, select **View**.
  3. To get day-to-day data for a metric, select the line-chart. 

#### Explore aggregated pipeline analytics (global view)
Here is an example of the aggregated pipeline view in the Home page. 

  {% include image.html 
  lightbox="true" 
  file="/images/pipeline/monitoring/monitor-aggregated-view.png" 
  url="/images/pipeline/monitoring/monitor-aggregated-view.png"
  alt="Global analytics for Delivery Pipelines in Home page"
  caption="Global analytics for Delivery Pipelines in Home page"
  max-width="30%"
  %}

The table describes the information for pipelines in the Home page.

{: .table .table-bordered .table-hover}
| Legend            | Description |  
| --------------    | ------------|  
| **1**             | The Home page with global Delivery Pipelines analytics.|                             
|**2**              | The filters by which to customize the aggregated view. For information on the filters, see [Filters quick reference](#filters-for-aggregated-views) in this article. All filters support multiple values.|          
|**3**              |  Line chart views of KPIs for Delivery Pipelines. Selecting a line chart displays the detailed day-to-day values for the metrics. For information on each metric, see [Metrics quick reference](#metrics-in-aggregated-views) in this article. |       
|**4**             | Pipelines by activity and duration. List views of up to ten of the most active, and ten of the longest running, pipelines. {::nomarkdown}<ul><li>Pipelines without numbers prefixing the names indicate new pipelines within the selected time frame.</li><li>Pipelines prefixed with numbers (encircled in the image above) indicate existing pipelines. </br> The number indicates the change in position of the pipeline compared to the reference period. </li><li>To drill down into a specific pipeline, select the pipeline.</li></ul> {:/}|    



#### Filters for aggregated pipeline analytics
Filters narrow the scope of aggregated data, allowing you to visualize just the information you want to see. Unless otherwise indicated, all filters support multi-selection.

{: .table .table-bordered .table-hover}
|  Filter          |  Description|  
| --------------   | --------------|  
| **Status**           | {::nomarkdown}<ul><li><b>Succeeded</b>: Pipelines with workflows completed successfully.</li> <li><b>Failed</b>: Pipelines with workflows that failed. </li><li><b>Error</b>: Pipelines with workflows that resulted in errors.</li> {:/}|                            
| **Repository**             |The Git repository or repositories tracked, with the events that triggered or ran the pipelines. |          
| **Event Type**       | The Git or Calendar event or events by which to view pipelines.  If you select Git push, only those pipelines configured to be run on Git push are displayed. |    
| **Initiator**       | The user who made the commit that triggered the event and caused the pipeline to run.|       

#### Metrics in aggregated pipeline analytics

Pipeline metrics (KPIs), are displayed as line charts and in list formats. 
* Line charts visualize the detailed day-to-day values for the metric in the selected time range.
* List formats display the average values for the metrics, and shows the fluctuations in the values compared to the reference time range. The reference time range is the period of time that corresponds to and precedes the selected time range. 

{: .table .table-bordered .table-hover}
|  Metric               |  Description|  
| --------------        | --------------|  
| **Success Rate**      | The average number of successful executions, in percentage. |                            
| **Average Duration**  | The average length of time to complete execution, in mm:ss. |          
| **Executions**        | The average number of times the pipeline was triggered, in percentage. |    
| **Committers**        | The number of users whose commits on the repository or repositories triggered the pipelines. The user count is aggregated per user, so if the same user made ten commits, the ten commits are counted as a single commit.|       


### Managing individual pipelines

Individual pipeline view shows all pipelines in your cluster, including those that are not active, without workflows.  

When you select an individual pipeline, you can monitor steps within workflows.  
Both step and workflow analytics share the same set of filters that allow you to reduce the data displayed and customize the view.


#### View individual pipeline analytics (list view)

1. In the Codefresh UI, go to the [Delivery Pipelines](https://g.codefresh.io/2.0/pipelines){:target="\_blank"} page.
1. To find specific pipelines, in the search field, enter a part of the pipeline name. For example, to find all CI-based pipelines, enter `ci`.

#### Explore pipeline list view

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
| **Recent Activity**| Bar chart of up to ten of the most recent workflows submitted for the pipeline. {::nomarkdown}<ul><li>Each bar represents a workflow, and is color-coded to indicate the workflow's status. Green is for completed, red for failed, and blue for ongoing workflows. </li><li> Mouse over a bar to see a pop-up with the metadata for the workflow. </li> <li>Selecting View Workflow Details takes you to the Workflow tab where you have several options to manage the workflow  and view logs. For more information, see [Managing workflows]({{site.baseurl}}/docs/pipelines/workflows).</li></ul> {:/}|          
| **Last Run**| The time of the most recent run of the pipeline, that is a workflow. The time is also color-coded to indicate the status of the workflow.  |       
| **Success Rate**| The number of workflows that completed successfully in the last seven days, and the comparison to the reference period in percentage. | 
| **Avg. Duration**| The average length of time for the pipeline's workflows to complete execution, in mm:ss. |   
| **Weekly Runs**| The number of times the pipeline was run and its workflows submitted in the last seven days, and the difference in percentage compared to the reference period. |  
| **Runtime**| The runtime in which the pipeline is run.  |  
| **Last Modified**| The date and time that the pipeline was recently updated. The updates include changes in configuration and manifests.|  
| **Sync Status**| The sync status with the desired state in Git.|  






### Update pipeline configuration
Update the Workflow Template, Trigger Conditions and arguments configured for the pipeline. 

> You can also update the manifest directly in the Manifests tab.

1. From either the **Home** or the **Delivery Pipelines** pages, select a pipeline.
1. Select the **Configuration** tab. 
1. To modify the Workflow Template, select **Workflow Templates**.
  The Workflow Template structure is entry point , followed by the list of Workflow Templates referenced with links.

1. Modify settings as needed, using the **Diff View** to see the changes. 
  Codefresh validates the settings in real-time alerting you to errors and warnings.
  > If there are Errors, Commit is disabled until you resolve the errors.



### Update pipeline manifests
Update the YAML manifests of the Workflow Template, event source and sensor configured for the pipeline. 
The YAML view shows the Git, desired, and live states of the manifests in parallel. Codefresh shows you three states to 
The Git state is the state-of-truth and is the only editable state. The desired state is the state after the changes are committed, before they are synced to the cluster. The live state is the actual state currently in the cluster. 


1. From either the **Home** or the **Delivery Pipelines** pages, select a pipeline.
1. Select the **Manifests** tab. 
1. To review the changes by manifest, select the 
1. From the drop-down list, select any YAML manifest.
1. Compare the Git, desired, and actual versions if needed.
1. To update, switch to the **Git State** and Select **Edit**. 
  
1. Modify settings as needed, using the **Diff View** to see the changes. 
  Codefresh validates the settings in real-time alerting you to errors and warnings.
  > If there are Errors, Commit is disabled until you resolve the errors.

### What to read next
[Workflows in pipelines]({{site.baseurl}}/docs/pipelines/workflows/)