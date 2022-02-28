---
title: "Pipeline monitoring"
description: ""
group: pipelines
toc: true
---

After creating a pipeline, CSDP starts collecting data when the pipeline runs and submits the workflow. Monitor running pipelines and manage them 


### Delivery Pipeline views
CSDP displays Delivery Pipeline activity in aggregated and individual view formats. 
* Aggregated: Global view of all the active pipelines in your cluster? or in your orgnaization. For high-level KPI metrics, use this view.  
* Individual: List of up to ?? pipelines with metrics for each pipeline including those that do not have any workflows, meaning those that are not triggered.



#### Filters quick reference
Both aggregated and individual pipeline views share the same set of filters. Filters allow you to create customized views with just the information you want to see. The defaukt Particuralty useful when you have literally hundreds of pipelines deployed across 

{: .table .table-bordered .table-hover}
|  Filter          |  Description|  
| --------------   | --------------           |  
| Status           | Succeeded: Pipelines with workflows completed successfully.
Failed: Pipelines with workflows that failed.
Error: Pipelines with workflows that resulted in errors. 

|                  |                            
| Repo             |The Git repository or repositories with the events that triggered or ran the pipelines. |          
| Event Type       | The Git or Calendar event or events by which to view pipelines.  If you select Git push, only those pipelines configured to be run on Git push are displayed. |    
| Initiator        | The user who made the commit that triggered the event and ran the pipeline.|       
| Time             |The time frame for which to show pipeline data, and can be `Last 7 days` (the default), `Last 30 days`, or `Last 90 days`|

#### Metrics quick reference 

KPIs for pipelines, in graphical and list formats. Selecting a mini-graph displays the detailed day-to-day chart metrics.
Success Rate: The average number of successful executions, in percentage. 

Average Duration: 
The average length of time to complete execution, in mm:ss. . 
Executions: The average number of times the pipeline was triggered, in percentage. 
Committers: The number of users whose commits on the repository or repositorues triggered the events configured for the pipelines. The user count is aggregated per user, so if the same user made ten commits, all commits are counted only once.



### Work with aggregated pipeline views



In the CSDP UI, go to the [Home](https://g.codefresh.io/2.0/) page.
Select filters to customize the aggregated view.
Select a mini-chart to expand the 
To drill down into a specufuc pipeline, from either the Most Active or Longest Delivy Pipeline lists, select a pipeline name.

  {% include image.html 
  lightbox="true" 
  file="/images/pipeline/monitoring/monitor-aggregated-view.png" 
  url="/images/pipeline/monitoring/monitor-aggregated-view.png"
  alt="Aggregated view for Delivery Pipelines in Home page"
  caption="Aggregated view for Delivery Pipelines in Home page"
  max-width="30%"
  %}

The table describes 

{: .table .table-bordered .table-hover}
| Legend        |  Description|  
| --------------| --------------           |  
| 1              | The Home page where aggregated views for Delivery Pipelines are displayed.|                             
| 2              | The filters by which to customize the aggregated view. For information on the filters, see [Filters quick reference] earlier in this article. Remember multi-select is supported for most filters, except the Time filter.|          
| 3              | The chart views or graphical represenations of KPIs for Delivery Pipelines. Selecting a graph shows the detailed day-to-day metrics. For information on each metric, see [Metrics quick reference] earlier in this article. |       
| 4             | Pipelines by activity and duration. List view of up to ten of the most active, and the longest running pipelines. To drill down, select a pipeline. |       

### Work with individual pipeline views
  * For individual pipeline view, go to the [Delivery Pipelines](https://g.codefresh.io/2.0/pipelines) page.
1. Set the filters:
  * For the aggregated view, select the filters. 
  * For the individual view, select the pipeline and then select the filters. 
1. To 

 The aggregated view is the global view of all the Delivery Pipelines currently running or active in your organization. It is the view on the Home page.
To get quick status use this view


View step analytics 
In the individual pipline view you can see analytics for each step in the pipeline. A step in a pipeline performs a task according to the flow defined in the workflow.
1. Select a pipeline.
1. Then select the **Dashboard** tab.

The lower half of the Dashboards page lists each step in the pipeline with these details:
Type: The result of step execution, which can be custom task performed by the step or K8s object or resource created by the step. For example, Pod. In the previous sections, you saw how to trigger the Argo workflows. In this tutorial, you will see how to trigger Pod and Deployment. Retry: The behavior if a previous step was not completed successfully.  a set of conditions that need to be satisfied in order to execute this step. You can find more information in th
Template: The tempkate the step references 
Workflow Template: The Workflow Template referenced from within the 
Avg. Duration: The length of time for the step to complete execution. The comparison metric shows the duration for the corresponding period of time, preceding the selected time frame.  
Executions: The average number of times the step was executed. As for the average duration, the comparison metric shows the performance for the corresponding period of time, preceding the selected time frame.
CPU and Mwmory
Errors


View workflow details
Once the sesnor trigger runs a pipeline, it submits a workflow. 
In the Pielines page select a pirpline and then select the Workflows tab

Bar chart shows the Mouse over the bar to see pop-up 


Manage/optimize workflows

Update workflow templates, arguments, trigger conditions

Update manifests






Aggregated versus individual views


Filters for customized data views

Pipeline metrics




