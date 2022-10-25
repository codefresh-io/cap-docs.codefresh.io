---
title: "Monitoring applications"
description: ""
group: deployment
toc: true
---

Monitor applications, deployments, application resources, and services in the Applications dashboard. As a one-stop shop for Argo Rollouts and Argo CD, the Applications dashboard in Codefresh delivers on the challenge of keeping track of your deployments, whatever the frequency and scale, across all clusters in your enterprise. A wide range of filters, progressive delivery views, and enriched CI and CD information, provide full traceability and visibility to your deployments. 



You can monitor different aspects of applications and deployments, including the application itself, its deployments, resources, and services.
* [Monitoring applications](#monitoring-applications)  
  Monitor  health and sync status of the application, andts resource . 
  Visibility into deployments from all the clusters associated with the provisioned runtimes, in-cluster and managed
* [Monitoring application resources](#monitoring-application-resources) 
  Monitor health, sync, manifests, logs, and events for each resource in the application. Take action on the resources such as rollouts    
* [Monitoring application deployments](#monitoring-application-deployments) 
  Monitor current and historical deployments.  
  View enriched CI information for each deployment, including links to container images, Git hashes correlated with feature requests, Jira issues and more.

>For information on creating and managing applications, see [Creating applications]({{site.baseurl}}/docs/deployment/create-application/) and [Managing applications]({{site.baseurl}}/docs/deployment/manage-application/).

### Select view mode for the Applications dashboard 
View deployed applications in either List (the default) or Card views. Both views are sorted by the most recent deployments. 

1. In the Codefresh UI, go to the [Applications dashboard](https://g.codefresh.io/2.0/applications-dashboard){:target="\_blank"}.
1. Select **List** or **Cards**.

#### Applications List view

Here is an example of the Applications dashboard in List view mode. 

{% include
image.html
lightbox="true"
file="/images/applications/app-dashboard-main-view.png"
url="/images/applications/app-dashboard-main-view.png"
alt="Applications Dashboard: List view"
caption="pplications Dashboard: List view"
max-width="30%"
%} 

<!---#### Applications Card view
Here is an example of the Applications dashboard in Card view mode. The Card view provides an scannable view of application data and the actions to manage applications. 

  {% include
image.html
lightbox="true"
file="/images/applications/app-dashboard-card-view.png"
url="/images/applications/app-dashboard-card-view.png"
alt="Applications Dashboard: Card view"
caption="pplications Dashboard: Card view"
max-width="30%"
%} -->

#### Application dashboard information
Here's a description of the information and actions available in the Applications dashboard.

{: .table .table-bordered .table-hover}
| Item                     | Description            |  
| --------------         | --------------           |  
|Application filters       | Filter by a range of attributes to customize the information in the dashboard to bring you what you need. {::nomarkdown}  <ul><li>Application state<br>The application state snapshot displays a breakdown of the deployed applications according to their state.<br>Click a state to filter by applications that match the state.</li><li>Application attributes<br>Attribute filters support multi-selection, and results are based on an OR relationship within the same filter with multiple options, and an AND relationship between filters.<br>Clicking <b>More Filters</b> gives you options to filter by application type, health, and labels. <br><ul><li>Application type: Applications and ApplicationSet </li><li>Health filters: The built-in Argo CD set of health filters. For more information, see the official documentation on <a href="https://argo-cd.readthedocs.io/en/stable/operator-manual/health" target=”_blank”>Health sets</a>.</li><li>Labels:The K8s labels defined for the applications. The list displays labels of <i>all</i> the applications, even if you have applied filters.<br>To see the available labels, select <b>Add</b>, and then select the required label and one or more values. <br>To filter by the labels, select <b>Add</b> and then <b>Apply</b>.<br> For more information, see the official documentation on <a href="https://kubernetes.io/docs/concepts/overview/working-with-objects/labels" target=”_blank”>Labels and selectors</a>.</li></ul></ul>{:/}|
|{::nomarkdown}<img src="../../../images/icons/icon-mark-favorite.png?display=inline-block">{:/}| Star applications as favorites and view only the starred applications.{::nomarkdown}<br>Select the <img src="../../../images/icons/icon-mark-favorite.png?display=inline-block"> to star the application as a favorite.<br><br>To filter by favorite applications, on the filters bar, select <img src="../../../images/icons/icon-fav-starred.png?display=inline-block">.<br>{:/} TIP: If you star applications as favorites in the Applications dashboard, you can filter by the same applications in the [DORA metrics dashboard]({{site.baseurl}}/docs/reporting/dora-metrics/#metrics-for-favorite-applications).  |
|Application label| Applications are labeled as one of the following:{::nomarkdown}<ul><li>Git Source App<br> The application includes other resources such as other applications, workflows, sensor, Delivery Pipelines.</li>{:/}See [Git Sources in runtimes]({{site.baseurl}}/docs/docs/runtime/git-sources).{::nomarkdown}<li>Application<br>The application is a standalone application.</li>{:/}For more information, see Argo CD's official documentation on [Applications](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#applications){:target="\_blank"}.{::nomarkdown}<li>ApplicationSet<br>The application is deployed as a set of applications, based on Argo CD's ApplicationSet CRD.</li>{:/}For more information, see Argo CD's official documentation on [Generating Applications with ApplicationSet](https://argo-cd.readthedocs.io/en/stable/user-guide/application-set/){:target="\_blank"}.{::nomarkdown}<li>App-of-apps<br>In this deployment model, the parent application deploys a set of child applications.</li>{:/}For more information, see Argo CD's official documentation on [App of Apps](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#app-of-apps){:target="\_blank"}.|
|Application actions| Options to monitor/manage the applications available in the context menu. {::nomarkdown}<ul><li>Quick view<br>A comprehensive read-only view of the deployment and definition information for the application.</li>{:/}See [Application Quick View](#application-quick-view) in this article.{::nomarkdown}<li>Synchronize<br>Manually synchronize the application.</li>{:/}See [Manually sync applications]({{site.baseurl}}/docs/deployment/sync-application).{::nomarkdown}<li>Edit<br>Modify application definitions.</li>{:/}See [Update application configuration]({{site.baseurl}}/docs/deployment/create-application/#update-application-configuration).{::nomarkdown}<li>Refresh and Hard Refresh: Available in Card view only. <ul><li>Refresh: Retrieve desired (Git) state, compare with the live (cluster) state, and refresh the application to sync with the desired state.</li><li>Hard Refresh: Refresh the application to sync with the Git sycn, while removing the cache.</li></ul>{:/} 




### Monitoring applications
The Applications dashbaord displays the applications that match the filter criteria you defined. 

You can: 

Identify errors for applications


#### Identify errors for applications
Errors in applications are flagged through the **Error** button, displayed at the top right of the Applications dashboard. Clicking the button shows the list of applications with the errors and the possible reasons for those errors.

{% include
image.html
lightbox="true"
file="/images/applications/app-dashboard-errors.png"
url="/images/applications/app-dashboard-errors.png"
alt="Error notifications in Applications Dashboard"
caption="Error notifications in Applications Dashboard"
max-width="50%"
%}

#### Monitor health and sync statuses for an application
Mointor the health of an application, its current sync status, and the result of the previous sync operation.  
Once you select an application, the quickest option to monitor statuses is through the application header which is always displayed, no matter what tab you navigate to.  



{% include
image.html
lightbox="true"
file="/images/applications/app-header-health-sync-status.png"
url="/images/applications/app-header-health-sync-status.png"
alt="Application header with health and sync status"
caption="Application header with health and sync status"
max-width="50%"
%}

Sync statuses also have **More** links that display details such as the date, tags, and message.

{% include
image.html
lightbox="true"
file="/images/applications/app-header-sync-details.png"
url="/images/applications/app-header-sync-details.png"
alt="Sync details panel on clicking More"
caption="Sync details panel on clicking More"
max-width="50%"
%}

>Tip:  
  You can also view the current health and sync status for the application resource in the Current State tab. 



#### View application details 

View deployment, definition, and event information for the selected application in a centralized location through the Quick View.  
A read-only view, the Quick View displays information on the application state and location, labels and annotations, parameters, sync options, manifest, status and sync events.
Access the Quick View from the Applications dashboard, either from the application's context menu, or after drilldown, from the Current State tab.

1. In the Codefresh UI, go to the [Applications dashboard](https://g.codefresh.io/2.0/applications-dashboard){:target="\_blank"}.
1. Do one of the following:  
  * From the List or Card views, select the context menu and then select **Quick View**.
  
{% include
image.html
lightbox="true"
file="/images/applications/quick-view-context-menu.png"
url="/images/applications/quick-view-context-menu.png"
alt="Selecting Quick View from the context menu"
caption="Selecting Quick View from the context menu"
max-width="50%"
%} 

  * Select the application, and from the application header's context menu on the right, select **Details**.

 {% include
image.html
lightbox="true"
file="/images/applications/app-header-view-details.png"
url="/images/applicationsapp-header-view-details.png"
alt="View app details from the application header context menu"
caption="View app details from the application header context menu"
max-width="50%"
%}

  * Select the application, and in the Current State tab, click the parent application resource.

{% include
image.html
lightbox="true"
file="/images/applications/quick-view-access-app-resource.png"
url="/images/applications/quick-view-access-app-resource.png"
alt="Accessing Quick View from the Current State tab"
caption="Accessing Quick View from the Current State tab"
max-width="50%"
%} 



##### Quick View: Summary
Displays health, sync status, and source and destination definitions.

{% include
image.html
lightbox="true"
file="/images/applications/quick-view-summary.png"
url="/images/applications/quick-view-summary.png"
alt="Application Quick View: Summary"
caption="Application Quick View: Summary"
max-width="40%"
%}

##### Quick View: Metadata
Displays labels and annotations for the application.

{% include
image.html
lightbox="true"
file="/images/applications/quick-view-metadata.png"
url="/images/applications/quick-view-metadata.png"
alt="Application Quick View: Metadata"
caption="Application Quick View: Metadata"
max-width="40%"
%}

##### Quick View: Parameters
Displays parameters configured for the application, based on the tool used to create the application's manifests.  
The parameters displayed differ according to the tool:  `directory` (as in the screenshot below), `Helm` charts, or `Kustomize` manifests, or the specific plugin.  

{% include
image.html
lightbox="true"
file="/images/applications/quick-view-parameters.png"
url="/images/applications/quick-view-parameters.png"
alt="Application Quick View: Parameters"
caption="Application Quick View: Parameters"
max-width="40%"
%}

##### Quick View: Sync Options
Displays sync options enabled for the application.

{% include
image.html
lightbox="true"
file="/images/applications/quick-view-parameters.png"
url="/images/applications/quick-view-parameters.png"
alt="Application Quick View: Parameters"
caption="Application Quick View: Parameters"
max-width="40%"
%}

##### Quick View: Manifest 
Displays the YAML version of the application manifest.

{% include
image.html
lightbox="true"
file="/images/applications/quick-view-manifest.png"
url="/images/applications/quick-view-manifest.png"
alt="Application Quick View: Manifest"
caption="Application Quick View: Manifest"
max-width="40%"
%}

##### Quick View: Events
Displays status and sync events for the application.

{% include
image.html
lightbox="true"
file="/images/applications/quick-view-events.png"
url="/images/applications/quick-view-events.png"
alt="Application Quick View: Events"
caption="Application Quick View: Events"
max-width="40%"
%}


### Monitoring application resources

Monitor the live state of the application's resources (Kubernetes objects) on the cluster, including health, sync state, manifests, and logs. Selecting an application from the Applications dashboard takes you to the Current State tab which displays the application's resources.  

{% include
image.html
lightbox="true"
file="/images/applications/app-resources-monitor-screen.png"
url="/images/applications/app-resources-monitor-screen.png"
alt="Monitor application resources in Current State tab"
caption="Monitor application resources in Current State tab"
max-width="50%"
%}

The icon for a resource node identifies the type of Kubernetes resource it represents. For general information on K8s resources, see [Working with Kubernetes objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/){:target="\_blank"}. 

You can:
* [View resources in List or Tree views](#view-modes-for-application-resources)
* Monitor health, sync, resource information:   
  * [Health status](#health-status-for-application-resources): Reflects Argo CD's built-in health checks and status for the Application resource and Kubernetes objects. For more information, see [Argo CD Resource Health](https://argo-cd.readthedocs.io/en/stable/operator-manual/health/){:target="\_blank"}.
  * [Sync status](#sync-status-for-application-resources): Desired state in Git versus live state in cluster.
  * Monitor resource manifests, logs, and events: 
    * [Manifests for application resources](#manifests-for-application-resources)
    * [Logs for application resources](#logs-for-application-resources)
    * [Manifests for application resources](#events-for-application-resources)


> To quickly see updated resources for the current or for a specific deployment, switch to the Timeline tab and expand the deployment entry to show Updated Resources. See [Monitor resource updates for deployments](#monitor-resource-updates-for-deployments).

#### View modes for application resources

The Current State tab supports Tree and List view formats. 
* Tree view (default): A hierarchical, interactive visualization of the application and its resources. Useful for complex deployments with multiple clusters and large numbers of resources. See also, [Working with resources in Tree view](#working-with-resources-in-tree-view).


{% include
image.html
lightbox="true"
file="/images/applications/current-state-tree-app-in-progress.png"
url="/images/applications/current-state-tree-app-in-progress.png"
alt="Tree view of application resources in Current State"
caption="Tree view of application resources in Current State"
max-width="50%"
%}

* List View: A list-based representation of application's resources, sorted by the Last Update. 
  Here is an example of the Current State in List view.

{% include
image.html
lightbox="true"
file="/images/applications/apps-current-state.png"
url="/images/applications/apps-current-state.png"
alt="List view of application resources in Current State"
caption="List view of application resources in Current State"
max-width="50%"
%}

> Filters are shared between Tree and List views, and when applied are retained when switching between views. 

##### Working with resources in Tree view
The Tree view is designed to impart key information at a glance. Review the sections that follow for pointers to get to what you need in the Tree view.

**Resource info**  
Mouse over a node to see a tooltip for that resource. For detailed information, select the resource; see [Detailed resource information](#detailed-resource-information) in this article.

{% include
image.html
lightbox="true"
file="/images/applications/current-state-resource-summary.png"
url="/images/applications/current-state-resource-summary.png"
alt="Current State Tree view: Resource tooltip"
caption="Current State Tree view: Resource tooltip"
max-width="50%"
%}

**Search resources**  
Quickly find a resource by typing the resource name in the search field. You can identify search results through the border which is different from the borders depicting health status. Press Enter to navigate to the next result. 

{% include
image.html
lightbox="true"
file="/images/applications/current-state-tree-search.png"
url="/images/applications/current-state-tree-search.png"
alt="Current State Tree view: Search resources"
caption="Current State: Search resources"
max-width="50%"
%}



**Resource inventory**   
The Resource inventory in the Tree view (bottom-left), summarizes the aggregated count for each resource type in the application.  
For visibility and quick access, `Syncing` and `Out-of-Sync` resources are bucketed separately. 

{::nomarkdown}
<br>
{:/}

Click-filters:  

In the resource inventory, selecting a `Syncing` or `Out-of-Sync` resource type, filters the Current State by that resource type and sync status.
These filters are automatically applied  to the default filter list for both Tree and List views. 
Here's an example of an application with out-of-sync resources, and the result on selecting an out-of-sync resource type.


{% include
image.html
lightbox="true"
file="/images/applications/current-state-tree-resource-list.png"
url="/images/applications/current-state-tree-resource-list.png"
alt="Current State Tree view: Resource inventory"
caption="Current State Tree view: Resource inventory"
max-width="50%"
%}

{% include
image.html
lightbox="true"
file="/images/applications/current-state-tree-resource-filtered.png"
url="/images/applications/current-state-tree-resource-filtered.png"
alt="Current State Tree view: Resource inventory filtered by out-of-sync service"
caption="Current State Tree view: Resource inventory filtered by out-of-sync service"
max-width="50%"
%}


#### Health status for application resources
View and monitor health status of the selected application's resources in the Current State tab, in Tree or List views.  
Identify the health of an application resource through the color-coded border and the resource-type icon (Tree view), or the textual labels at the right of the resource (List view).  


{: .table .table-bordered .table-hover}
| Health status   | Description | Display in Tree view  | 
| --------------  | ------------| ------------------|  
| **Healthy**     | Resource is functioning as required. | {::nomarkdown}<img src="../../../images/icons/current-state-healthy.png" display=inline-block">{:/} | 
| **Progressing** | Resource is not healthy but can become healthy.| {::nomarkdown}<img src="../../../images/icons/current-state-progressing.png" display=inline-block">{:/} | 
| **Suspended**   | Resource is not functioning, and is either suspended or paused. For example, Cron job or a canary rollout.| {::nomarkdown}<img src="../../../images/icons/current-state-suspended.png" display=inline-block">{:/} | 
| **Missing**     | Resource is not present on the cluster. |{::nomarkdown}<img src="../../../images/icons/current-state-missing.png" display=inline-block">{:/} |                        
| **Degraded**    | Resource is not healthy, or a timeout occured before it could reach a healthy status.| {::nomarkdown}<img src="../../../images/icons/current-state-degraded.png" display=inline-block/>{:/} |
| **Unknown**   | Resource does not have a health status or the health status is not tracked in Argo CD. For example,`ConfigMaps`.   | {::nomarkdown}<img src="../../../images/icons/current-state-unknown.png" display=inline-block">{:/} | 

See also [Argo CD's set of health checks](https://argo-cd.readthedocs.io/en/stable/operator-manual/health/).



#### Sync status for application resources

Similar to the health status, view and monitor the sync status of an application resource also in the Current State tab, in  Tree or List views. The sync status identifies if the live state of the application resource on the cluster is synced with its desired state in Git.   
Identify the sync status through the icon on the left of the resource name and the color of the resource name (Tree view), or the textual labels at the right of the resource (List view). 

The table describes the possible sync statuses for an application resource, and its representation in the Tree view. 

{: .table .table-bordered .table-hover}
| Sync state     | Description |Display in Tree view  |  
| -------------- | ----------    | ----------     |  
| **Synced**       | The live state of the resource on the cluster is identical to the desired state in Git.| {::nomarkdown}<img src="../../../images/icons/current-state-synced.png" display=inline-block">{:/} |                            
| **Syncing**      | The live state of the resource was not identical to the desired state, and is currently being synced.| {::nomarkdown}<img src="../../../images/icons/current-state-syncing.png" display=inline-block/>{:/} |  
| **Out-of-Sync**  | The live state is not identical to the desired state. | {::nomarkdown}<img src="../../../images/icons/current-state-out-of-sync.png" display=inline-block">{:/} |  
| **Unknown**      | The sync status could not be determined. | {::nomarkdown}<img src="../../../images/icons/current-state-sync-unknown.png" display=inline-block">{:/} |  

#### Manifests for application resources

In either Tree or List views, double-click an appplication resource to see its manifests. The manifests are displayed in the Summary tab. 
> Based on the selected resource type, you can also view logs, and events. Endpoints for example show only manifests, while pods show manifests, logs, and events.  

> To view information for the application resource, select the application node in Tree View. See [Application information](#application-quick-view).

 
{% include
image.html
lightbox="true"
file="/images/applications/current-state-resource-summary.png"
url="/images/applications/current-state-resource-summary.png"
alt="Current State Tree view: Resource tooltip"
caption="Current State Tree view: Resource tooltip"
max-width="50%"
%}

Here's what you can see and do in the Summary tab:
> Press Ctrl/Command F to search for strings in the manifest.

* Desired and Live states of the resource manifest: 
  * Managed resources, stored in Git repositories and using Git as the single source of truth, show both the Desired (Git) and the Live (cluster) states.    
    If there are discrepancies between them, the Diff view is displayed, highlighting the differences in both versions for comparison.
  * Resources that are not stored in Git but live in the cluster, show only the Live state.
* Share resource details: Copy the URL and send to others in your organization to share the resource details for collaborative review and analysis. Pasting the URL in a browser opens to the same view of the resource.
* Hide Managed Fields: In the Live state version of the manifest, you can hide managed-field information from the manifest. Managed-fields show information on which field manager manages the field, after Kubernetes introduced `Server Side Apply`. For more information, see [Field Management](https://kubernetes.io/docs/reference/using-api/server-side-apply/#field-management){:target="\_blank"}.

{::nomarkdown}
<br>
{:/}

#### Logs for application resources
In either Tree or List views, double-click an appplication resource to see its logs. Logs are available only for resource types such as pods.

{% include
image.html
lightbox="true"
file="/images/applications/current-state-logs.png"
url="/images/applications/current-state-logs.png"
alt="Current State: Logs for resource"
caption="Current State: Logs for resource"
max-width="50%"
%}


* Search: Free-text search for any string in the log, using the next and previous buttons to navigate between the results, or Enter for sequential navigation.
* Wrap: Enable/disable line wrapping 
* Download: Download the complete log into a text file for offline viewing and analysis.

{::nomarkdown}
<br>
{:/}

#### Events for application resources
In either Tree or List views, double-click an appplication resource to see events in the Events tab. 
> If your rutime is lower than the version required to view events, you are notified to upgrade to the required version.

The Events tab displays both successful and failed events from Argo CD, starting with the most recent event. 
Argo CD displays events as they occur for an application resource, and retains event information for a duration of 30 minutes. Historical events older than this duration are removed, and the Events tab can be empty if there are no ongoing events.

{% include
image.html
lightbox="true"
file="/images/applications/current-state-events-tab.png"
url="/images/applications/current-state-events-tab.png"
alt="Current State: Events for resource"
caption="Current State: Events for resource"
max-width="50%"
%}






### Monitoring application deployments  
Monitor ongoing deployments and review historical deployments for the selected application. 
The Timeline tab displays the history of deployments for the selected application, sorted by the most recent deployment (default), labelled **Current Version** at the top. 

{% include
image.html
lightbox="true"
file="/images/applications/dashboard-timeline-main.png"
url="/images/applications/dashboard-timeline-main.png"
alt="Applications Dashboard: Timeline tab"
caption="Applications Dashboard: Timeline tab"
max-width="30%"
%}

You can:

* Monitor CI information for deployments  
  Each deployment record is enriched with CI build, Pull Request (PR), Jira, and commit information, serving as an audit trail for visbility into all aspects and actions leading up to the deployment.   

* Deployment breakdown
  For every deployment, Codefresh displays a detailed breakdown of the following: 
  * Updated Kubernetes services 
  * Updated Argo resources
  * Updated applications

* Deployment chart 
  The deployment chart displays the day-to-day deployments for the selected time period. Mouse over the dot on the deployment chart for information on historical deployments.

**Deployment
Visulaize rollout steps
Manage rolloiuts

**How to monitor deployments**
1. If required, set filters to narrow the number of deployments for the selected application.
1. To view GitOps details for a deployment, mouse over the dot that represents the deployment. 
1. Expand a deployment entry to view details on the deployment.

{% include
image.html
lightbox="true"
file="/images/applications/apps-historical-deployment.png"
url="/images/applications/apps-historical-deployment.png"
alt="Applications Dashboard: Deployment chart"
caption="Applications Dashboard: Deployment chart"
max-width="30%"
%}

#### Monitor CI details by deployment
  

Each deployment record displays the complete CI history for that deployment.


{% include
image.html
lightbox="true"
file="/images/applications/app-dashboard-time-expanded-card.png"
url="/images/applications/app-dashboard-time-expanded-card.png"
alt="Applications Dashboard: Deployment record for selected application in Timeline tab"
caption="Applications Dashboard: Deployment record for selected applicatio in Timeline tab"
max-width="70%"
%}

* The **CI Builds** shows the image(s) created or updated during deployment. Click to see the **Images** view in a new browser window.
* The **Pull Request (PRs)** used for the commit.
* The Jira **Issues** the PR aims to resolve or has resolved, with the current status.
* The **Committer** who made the changes.


#### Monitor updated resources by deployment
The deployment record also identifies the resources that were changed (created, updated, or removed), and a diff view of the actual changes in Updated Resources.  
View changed  the Full or Compact views to see changes. The Full View shows the complete resource manifest, with a diff view of the changes, while the Compact View shows only the lines with the change. 

> For detailed information on the resource, switch to the Current State tab and click the resource node. See [Monitoring application resources](#monitoring-application-resources).

1. Select a deployment record, and expand **Updated Resources**.

{% include
image.html
lightbox="true"
file="/images/applications/timeline-resources-updated.png"
url="/images/applications/timeline-resources-updated.png"
alt="Timeline tab: Updated Resources for deployment"
caption="Timeline tab: Updated Resources for deployment"
max-width="70%"
%}

{:start="2"}
1. To see the changes for a resource, click the resource name.
  The Full View of the resource manifest is displayed with the diff view of the changes. 

{% include
image.html
lightbox="true"
file="/images/applications/timeline-resources-full-view.png"
url="/images/applications/timeline-resources-full-view.png"
alt="Full View of changes for updated resource"
caption="Full View of changes for updated resource"
max-width="70%"
%}

{:start="3"}
1. To view only the changes, click **Compact View**.

{% include
image.html
lightbox="true"
file="/images/applications/timeline-resources-compact-view.png"
url="/images/applications/timeline-resources-compact-view.png"
alt="Full View of changes for updated resource"
caption="Full View of changes for updated resource"
max-width="70%"
%}




#### Monitor rollouts by deployment
A rollout is initiated when there is an Argo CD sync due to a change in the desired state.  
Visualize completed rollouts, and ongoing rollouts in the deployment record in **Services**.  
The rollout progress bar indicates the status of the rollout. For an ongoing rollout, you can visualize the steps in the rollout and control them using the options in the Rollout Player.  


> To view and manage a rollout, you must have an Argo `rollout` resource defined for your application, and [install Argo Rollouts in the cluster]({site.baseurl}}/docs/_docs/deployment/install-argo-rollouts). 

For detailed information on Argo Rollouts, see [Argo Rollouts documentation](https://argoproj.github.io/argo-rollouts/){:target="\_blank"}.

##### Visualize rollout progress
Here is an example of an ongoing rollout for a canary deployment in Timeline > Services  The rollout comprising four steps has not started, and no traffic has not been routed as yet to the new version of the application.

{% include
image.html
lightbox="true"
file="/images/applications/apps-dashboard-rollout-in-progress.png"
url="/images/applications/apps-dashboard-rollout-in-progress.png"
alt="Application deployment in progress"
caption="Application deployment in progress"
max-width="50%"
%}

Here is an example of the rollout progress for the same deployment on completion. All traffic has been routed to the new version. 

{% include
image.html
lightbox="true"
file="/images/applications/apps-dashboard-rollout-complete.png"
url="/images/applications/apps-dashboard-rollout-complete.png"
alt="Application deployment completed"
caption="Application deployment completed"
max-width="50%"
%}

##### Manage ongoing rollout
Click the rollout name to visualize its steps. Manually manage the rollout by skipping a specific step, resuming a paused step, or promoting a full rollout through the options in the Rollout Player. 
Here you can see that two out of four steps have been completed, 25% of the traffic has been routed, and the rollout has been paused for the defined length of time. 

{% include
image.html
lightbox="true"
file="/images/applications/apps-dashboard-rollout-analysis.png"
url="/images/applications/apps-dashboard-rollout-analysis.png"
alt="Application rollout: Analysis run"
caption="Applications rollout: Analysis run"
max-width="30%"
%}

The Rollout Player shows the options to manually manage the rollout.

{% include
image.html
lightbox="true"
file="/images/applications/rollout-player.png"
url="/images/applications/rollout-player.png"
alt="Rollout step visualization and Rollout Player"
caption="Rollout steps and Rollout Player"
max-width="50%"
%}

 
The table lists the options in the Rollout Player to manage an onging rollout.

: .table .table-bordered .table-hover}
| Rollback player option   | Description |  
| --------------  | ------------| 
| **Rollback**      | Not implemented currently.  | 
| **Resume** {::nomarkdown}<img src="../../../images/icons/rollout-resume.png" display=inline-block"> {:/}| Resume a paused step without waiting for the period of time defined as the pause duration. | 
| **Skip step** {::nomarkdown}<img src="../../../images/icons/rollout-skip-step.png" display=inline-block"> {:/}  | Skip execution of current step. Skipped steps are marked as Skipped in the rollout visualization. | 
| **Promote full rollout** {::nomarkdown}<img src="../../../images/icons/rollout-promote-full.png" display=inline-block"> {:/}   | Skip remaining pause, traffic routing, and analysis steps, and deploy the current release. |                        


 
##### View analyis template
If you have defined an analysis template for the rollout, you can check the run results and the manifest. 
 The result of an analysis run determines if the rollout is completed, paused, or aborted. For detailed information, see the [Analysis section in Argo Rollouts](https://argoproj.github.io/argo-rollouts/features/analysis/){:target="\_blank"}.  

If you are running Background Analysis for example, the first step shows the list of analysis metrics.

{% include
image.html
lightbox="true"
file="/images/applications/app-rollout-analysis-template-step.png"
url="/images/applications/app-rollout-analysis-template-step.png"
alt="Rollout: Analaysis Metrics in Background Analysis"
caption="Analysis Template: Analaysis Metrics in Background Analysis"
max-width="50%"
%}

Click the metric link in the step. If you are running Background Analysis, this is the first step.

{% include
image.html
lightbox="true"
file="/images/applications/app-rollout-run-results-manifest.png"
url="/images/applications/app-rollout-run-results-manifest.png"
alt="Analysis Template: Run Results and Manifest for Analysis Metric"
caption="Analysis Template: Run Results and Manifest for Analysis Metric"
max-width="50%"
%}


### Monitor services by deployment
The Services tab for  shows the K8s services for each deployment of the application. 
Each service shows the number of replicas, the endpoint IP, the labels that reference the application, and the health status.  

For more information, see the official documentation on [Services](https://kubernetes.io/docs/concepts/services-networking/service/){:target="\_blank"}.

{% include
image.html
lightbox="true"
file="/images/applications/apps-dashboard-services.png"
url="/images/applications/apps-dashboard-services.png"
alt="Applications Dashboard: Services tab"
caption="Applications Dashboard: Services tab"
max-width="50%"
%}




