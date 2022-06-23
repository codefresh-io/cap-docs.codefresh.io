---
title: "Managing pipelines"
description: ""
group: pipelines
toc: true
---

Once a Delivery Pipeline is [created and triggered]({{site.baseurl}}/docs/pipelines/create-pipeline/),Codefresh continuously collects and displays real-time analytics on the pipeline and the workflows submitted.  
>This article focuses on pipeline management. For information on managing workflows in pipelines, see [Managing workflows]({{site.baseurl}}/docs/pipelines/workflows/).


Manage Delivery Pipelines in Codefresh by monitoring pipeline performance and analytics, and by optimizing pipeline configurations and manifests. 


**Monitoring pipeline performance and analytics**  
Monitor pipelines performance at the global and individual levels. Both levels provide metrics for KPIs for insights into trends and performance.

* At the global level (Home dashboard), Codefresh aggregates performance analytics for all pipelines with active workflows.
* At the individual level (Delivery Pipelines list), Codefresh aggregates performance based on the pipeline's workflows, including step analytics across all the workflows in the pipeline. Step analytics allow you to analyze the resources consumed by a pipeline.
* Monitoring sensor and event-source errors for  


**Optimizing pipeline configuration**
Use insights and trends from performance and KPI metrics to optimize the pipeline's Workflow Template, trigger conditions, and arguments. 







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

### Monitor global pipeline performance 
Monitor performance of all active pipelines in the Home dashboard. Active pipelines are those that have at least one active or complete workflow.  
The KPI metrics provides insights into trends for pipelines. Analytics are derived by comparing the selected time range to the corresponding reference period. If your time range is the last seven days, the reference period is the seven days that precede the time range. 


1. In the Codefresh UI, go to the [Home](https://g.codefresh.io/2.0/){:target="\_blank"} page.
1. Set the **Time** frame for which to show pipeline data, as the **Last 7 days** (the default), **Last 30 days**, or **Last 90 days**.
1. Scroll down to Delivery Pipelines:
  1. To customize the view, select **Filters**.
  2. To switch to individual pipeline view, select **View**.
  3. To get day-to-day data for a metric, select the line-chart. 

Here is an example of the global pipeline view in the Home page. 

  {% include image.html 
  lightbox="true" 
  file="/images/pipeline/monitoring/monitor-aggregated-view.png" 
  url="/images/pipeline/monitoring/monitor-aggregated-view.png"
  alt="Global analytics for Delivery Pipelines in Home page"
  caption="Global analytics for Delivery Pipelines in Home page"
  max-width="30%"
  %}




#### Filters for global pipeline view
Filters narrow the scope of aggregated data, allowing you to focus on the information you want to see. Unless otherwise indicated, all filters support multi-selection.

{: .table .table-bordered .table-hover}
|  Filter          |  Description|  
| --------------   | --------------|  
| **Status**           | {::nomarkdown}<ul><li><b>Succeeded</b>: Pipelines with workflows completed successfully.</li> <li><b>Failed</b>: Pipelines with workflows that failed. </li><li><b>Error</b>: Pipelines with workflows that resulted in errors.</li> {:/}|                            
| **Repository**             |The Git repository or repositories tracked, with the events that triggered or ran the pipelines. |          
| **Event Type**       | The Git or Calendar event or events by which to view pipelines.  If you select Git push, only those pipelines configured to be run on Git push are displayed. |    
| **Initiator**       | The user who made the commit that triggered the event and caused the pipeline to run.|       

#### Metrics in global pipeline view

Pipeline metrics (KPIs), are displayed as line charts and in list formats. 
* Line charts
  Quick views of KPIs for the selected time frame. To see detailed day-to-day values, select a line chart.
* List formats
  Display the average values for the same KPIs, sorted by activity and duration. The different perspectives illustrate both the fluctuations in the KPIs compared to the reference time range, and trending pipelines. The reference time range is the period of time that corresponds to and precedes the selected time range. 

  {::nomarkdown}<ul><li>Pipelines without numbers prefixing the names indicate new pipelines within the selected time frame.</li><li>Pipelines prefixed with numbers (encircled in the image above) indicate existing pipelines. </br> The number indicates the change in position of the pipeline compared to the reference period. </li><li>To drill down into a specific pipeline, select the pipeline.</li></ul>

{: .table .table-bordered .table-hover}
|  Metric               |  Description|  
| --------------        | --------------|  
| **Success Rate**      | The average number of successful executions, in percentage. |                            
| **Average Duration**  | The average length of time to complete execution, in mm:ss. |          
| **Executions**        | The average number of times the pipeline was triggered, in percentage. |    
| **Committers**        | The number of users whose commits on the repository or repositories triggered the pipelines. User count is aggregated per user, so multiple commits from the same user are counted as a single commit.|       


### Monitor individual pipeline performance

Monitor and compare KPIs for all pipelines in your cluster, including those that are not active, meaning pipelines for which workflows have not been submitted.  

View pipeline activity for the last seven days, the status and the sync state. Compare KPIs for all the pipelines in the cluster, and identify inactive pipelines.
 
1. In the Codefresh UI, go to the [Delivery Pipelines](https://g.codefresh.io/2.0/pipelines){:target="\_blank"} page.
1. To find specific pipelines, in the search field, enter a part of the pipeline name. For example, to find all CI-based pipelines, enter `ci`.

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
| **Name**      | The name of the pipeline. Select the name to drill down into step analytics, workflows, and configuration.|                             
| **Recent Activity**| Bar chart of up to ten of the most recent workflows submitted for the pipeline. {::nomarkdown}<ul><li>Each bar represents a workflow that is color-coded to indicate the workflow's status: Green for completed, red for failed, and blue for ongoing workflows. </li><li> Mouse over a bar to see a pop-up with the metadata for the workflow. </li> <li>Selecting View Workflow Details takes you to the Workflow tab where you have several options to manage the workflow and view logs. For more information, see <a href="https://codefresh.io/csdp-docs/docs/pipelines/workflows">Managing workflows</a>.</li></ul> {:/}|          
| **Last Run**| The time of the most recent run of the pipeline, when a workflow was submitted. The time is also color-coded to indicate the status of the workflow. Empty Last Run columns identify pipelines that have not been triggered to submit workflows.|       
| **Success Rate**| The number of workflows that completed successfully in the last seven days, and the comparison to the reference period in percentage. | 
| **Avg. Duration**| The average length of time for the pipeline's workflows to complete execution, in mm:ss. |   
| **Weekly Runs**| The number of times the pipeline was run and its workflows submitted in the last seven days, and the difference in percentage compared to the reference period. |  
| **Runtime**| The runtime in which the pipeline is run.  |  
| **Last Modified**| The date and time that the pipeline was recently updated. The updates include changes in configuration and manifests.|  
| **Sync Status**| The sync status of the live state in the cluster with the desired state in Git.|  



### Update pipeline configuration settings or manifests
Update the pipeline's Workflow Template, arguments, sensor and event source manifests to optimize or fix performance issues. 
You can update the settings in the Configuration tab or in the Manifests tab, working in the mode that you are comfortable with. 
* The Configuration tab shows a modular view of the settings in form mode, and makes for easier editing.  
* The Manifests tab shows the YAMLs generated on commit. Here you can see the Git, desired, and live states of each manifest in parallel.  
   The Git state is the state-of-truth and is the only editable state. The desired state is the state after the changes are committed, before they are synced to the cluster. The live state is the actual state currently in the cluster. 

For information on the settings, review [Delivery Pipeline creation flow]({{site.baseurl}}/docs/pipelines/create-pipeline/#delivery-pipeline-creation-flow)

1. From either the **Home** or the **Delivery Pipelines** pages, select a pipeline.
1. Select the **Configuration** tab. 
  * Select the component, and modify settings as needed. 
  * For the Workflow Template, use the **Diff View** to see changes. 

{:start="3"}
1. Select the **Manifests** tab.
  * Check if there are Errors or Warnings for the manifest.
  * To review the changes by manifest, from the drop-down list, select any YAML manifest.
  * Compare the Git, desired, and actual versions if needed.
  * To update, switch to the **Git State** and Select **Edit**. 
  * To view commit history, select **Sync History**.  
    Codefresh validates the settings in real-time alerting you to errors and warnings.
  
  > If there are Errors, Commit is disabled until you resolve the errors.

### What to read next
[Managing workflows]({{site.baseurl}}/docs/pipelines/workflows/)