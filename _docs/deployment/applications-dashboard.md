---
title: "Monitoring applications"
description: ""
group: deployment
toc: true
---

View, monitor, and analyze deployments across your enterprise in the Applications dashboard. As a one-stop shop for Argo Rollouts and Argo CD, the Applications dashboard in Codefresh delivers on the challenge of keeping track of your deployments, whatever the frequency and scale. A wide range of filters, progressive delivery views, and enriched CI and CD information, provide full traceability and visibility to your deployments. 

What can you do in the Applications dashbaord?
Select the view
Customize the view
View application information
Monitor application resources
Monitor application deployments
Monitor application services

Here are some insights you can derive from the Applications dashboard: 
* Application   
* Visibility into deployments from all the clusters associated with the provisioned runtimes, in-cluster and managed
* Timeline on current and historical deployments 
* Enriched CI information for deployments, including links to container images, Git hashes correlated with feature requests, Jira issues
* Microservices deployed by the application
* Hierarchical view of the resources in the application in the Current State

>For information on creating and managing applications, see [Creating applications]({{site.baseurl}}/docs/deployment/create-application/) and [Managing applications]({{site.baseurl}}/docs/deployment/manage-application/).

### Select the view mode for the Applications dashboard 
View deployed applications in either List (the default) or Card views. Both views are sorted by the most recent deployments by default. 

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

#### Applications Card view
Here is an example of the Applications dashboard in Card view mode.

  {% include
image.html
lightbox="true"
file="/images/applications/app-dashboard-card-view.png"
url="/images/applications/app-dashboard-card-view.png"
alt="Applications Dashboard: Card view"
caption="pplications Dashboard: Card view"
max-width="30%"
%} 

#### Application dashboard
Here's a description of the information and actions in the Applications dashboard.

{: .table .table-bordered .table-hover}
| Item                     | Description            |  
| --------------         | --------------           |  
|Application filters       | Filter by a range of attributes to customize the information in the dashboard to bring you what you need. {::nomarkdown}  <ul><li>Application state<br>The application state snapshot displays a breakdown of the deployed applications according to their state.<br>Click a state to filter by applications that match the state.</li><li>Application attributes<br>Attribute filters support multi-selection, and results are based on an OR relationship within the same filter with multiple options, and an AND relationship between filters.<br>Clicking <b>More Filters</b> gives you options to filter by application type, health, and labels. <br><ul><li>Application type: Applications and ApplicationSet </li><li>Health filters: The built-in Argo CD set of health filters. For more information, see the official documentation on <a href="https://argo-cd.readthedocs.io/en/stable/operator-manual/health" target=”_blank”>Health sets</a>.</li><li>Labels:The K8s labels defined for the applications. The list displays labels of <i>all</i> the applications, even if you have applied filters.<br>To see the available labels, select <b>Add</b>, and then select the required label and one or more values. <br>To filter by the labels, select <b>Add</b> and then <b>Apply</b>.<br> For more information, see the official documentation on <a href="https://kubernetes.io/docs/concepts/overview/working-with-objects/labels" target=”_blank”>Labels and selectors</a>.</li></ul></ul>{:/}|
|{::nomarkdown}<img src="../../../images/icons/icon-mark-favorite.png?display=inline-block">{:/}| Star applications as favorites and view only the starred applications.{::nomarkdown}<br>Select the <img src="../../../images/icons/icon-mark-favorite.png?display=inline-block"> to star the application as a favorite.<br><br>To filter by favorite applications, on the filters bar, select <img src="../../../images/icons/icon-fav-starred.png?display=inline-block">.<br>{:/} TIP: If you star applications as favorites in the Applications dashboard, you can filter by the same applications in the [DORA metrics dashboard]({{site.baseurl}}/docs/reporting/dora-metrics/#metrics-for-favorite-applications).  |
|Application label| Applications are labeled as one of the following:{::nomarkdown}<ul><li>Git Source App<br> The application includes other resources such as other applications, workflows, sensor, Delivery Pipelines.</li>{:/}See [Git Sources in runtimes]({{site.baseurl}}/docs/docs/runtime/git-sources).{::nomarkdown}<li>Application<br>The application is a standalone application.</li>{:/}For more information, see Argo CD's official documentation on [Applications](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#applications){:target="\_blank"}.{::nomarkdown}<li>ApplicationSet<br>The application is deployed as a set of applications, based on Argo CD's ApplicationSet CRD.</li>{:/}For more information, see Argo CD's official documentation on [Generating Applications with ApplicationSet](https://argo-cd.readthedocs.io/en/stable/user-guide/application-set/){:target="\_blank"}.{::nomarkdown}<li>App-of-apps<br>In this deployment model, the parent application deploys a set of child applications.</li>{:/}For more information, see Argo CD's official documentation on [App of Apps](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#app-of-apps){:target="\_blank"}.|
|Application actions| Options to monitor/manage the applications available in the context menu. {::nomarkdown}<ul><li>Quick view<br>A comprehensive read-only view of the deployment and definition information for the application.</li>{:/}See [Application Quick View](#application-quick-view) in this article.{::nomarkdown}<li>Synchronize<br>Manually synchronize the application.</li>{:/}See [Manually sync applications]({{site.baseurl}}/docs/deployment/sync-application).{::nomarkdown}<li>Edit<br>Modify application definitions.</li>{:/}See [Update application configuration]({{site.baseurl}}/docs/deployment/create-application/#update-application-configuration).{::nomarkdown}<li>Refresh and Hard Refresh: Available in Card view only. <ul><li>Refresh: Retrieve desired (Git) state, compare with the live (cluster) state, and refresh the application to sync with the desired state.</li><li>Hard Refresh: Refresh the application to sync with the Git sycn, while removing the cache.</li></ul>{:/} 




### Monitoring applications

#### Identifying errors in applications
Errors in applications are flagged through the **Error Detected** button on the top right of the Applications dashboard. Clicking the button shows the list of applications with the errors and the possible reasons for those errors.

{% include
image.html
lightbox="true"
file="/images/applications/app-dashboard-errors.png"
url="/images/applications/app-dashboard-errors.png"
alt="Error notifications in Applications Dashboard"
caption="Error notifications in Applications Dashboard"
max-width="50%"
%}


#### View application information

View deployment, definition, and event information for the selected application.  
The Quick View is a read-only view that displays in a centralized location information at the application level: application state and location, labels and annotations, parameters, sync options, manifest, status and sync events.
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



#### Quick View: Summary
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

#### Quick View: Metadata
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

#### Quick View: Parameters
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

#### Quick View: Sync Options
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

#### Quick View: Manifest 
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

#### Quick View: Events
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

Monitor the live state of the application's resources (Kubernetes objects) on the cluster, including health, sync state, manifest and logs. 
Drill down into the application from the Applications dashboard to the Current State tab to view and monitor resources for the selected application. 

**Insights and actions for application resoruces in Current State**
*  Health status: Reflects Argo CD's built-in health checks and status for the Application resource and Kubernetes objects. For more information, see [Argo CD Resource Health](https://argo-cd.readthedocs.io/en/stable/operator-manual/health/){:target="\_blank"}.
*  Sync state: Desired state in Git versus live state in cluster.
* Resource details: High-level information on mouse-over, and detailed manifest and log information on selecting the resource.


The icon for a resource node identifies the type of Kubernetes resource it represents. For general information on K8s resources, see [Working with Kubernetes objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/){:target="\_blank"}.

#### View modes for application resources

The Current State tab supports Tree and List view formats. 
* Tree view (default): A hierarchical, interactive visualization of the application and its resources. Useful for complex deployments with multiple clusters and large numbers of resources.  


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

#### Monitor sync states for application resources
Monitor an as the application node displays Selecting a resource, in either Tree or List view shows resource manifests, logs, and events if any, based on the resource type you selected. Endpoints for example show only manifests, while pods show manifests, logs, and events.  

> Selecting the application resource in Tree View, displays information for the [application](#application-quick-view).

{::nomarkdown}
<br>
{:/}

**Summary**
 
{% include
image.html
lightbox="true"
file="/images/applications/current-state-resource-summary.png"
url="/images/applications/current-state-resource-summary.png"
alt="Current State Tree view: Resource tooltip"
caption="Current State Tree view: Resource tooltip"
max-width="50%"
%}

* Desired and Live states: 
  * Managed resources, stored in Git repositories and using Git as the single source of truth, show both the Desired and the Live states.    
    If there are discrepancies between them, the Diff view is displayed, highlighting the differences in both versions for comparison.
  * Resources that are not stored in Git but live in the cluster, show only the Live state.
* Share resource details: Copy the URL and send to others in your organization to share the resource details for collaborative review and analysis. Pasting the URL in a browser opens to the same view of the resource.
* Hide Managed Fields: When selected, hides managed-field information from the manifest. Managed-fields show information on which field manager manages the field, after Kubernetes introduced `Server Side Apply`. For more information, see [Field Management](https://kubernetes.io/docs/reference/using-api/server-side-apply/#field-management){:target="\_blank"}.

{::nomarkdown}
<br>
{:/}

#### View and analyze logs for application resources

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

#### View and analyze events for application resources

{% include
image.html
lightbox="true"
file="/images/applications/current-state-events-tab.png"
url="/images/applications/current-state-events-tab.png"
alt="Current State: Events for resource"
caption="Current State: Events for resource"
max-width="50%"
%}

View events for application resources in the Events tab.  
> If your rutime is lower than the version required to view events, you are notified to upgrade your runtime.

The Events tab displays both successful and failed events from Argo CD, starting with the most recent event. 
Argo CD displays events as they occur for an application resource, and retains event information for a duration of 30 minutes. Historical events older than this duration are removed, and the Events tab can be empty if there are no ongoing events.



#### Working with application resources in Tree view
The Tree view is designed to impart key information at a glance. Review the sections that follow for pointers to get to what you need in the Tree view.

##### Resource info
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

##### Search resources
Quickly find a resource by typing the resource name in the search field. Search results have a different border for identification. Press Enter to navigate to the next result. 

{% include
image.html
lightbox="true"
file="/images/applications/current-state-tree-search.png"
url="/images/applications/current-state-tree-search.png"
alt="Current State Tree view: Search resources"
caption="Current State: Search resources"
max-width="50%"
%}

##### Resource health status in Tree view  
Identify the health of an application resurce through the color-coded border and the resource-type icon. The health statuses tracked are [Argo CD's set of health checks](https://argo-cd.readthedocs.io/en/stable/operator-manual/health/).

> Resources without a health status or with health statuses not tracked in Argo CD, such as `ConfigMaps` for example, are assigned the `Unknown` health status.

{: .table .table-bordered .table-hover}
| Health status   | Display in Tree view  | 
| --------------  | --------------------|  
| **Healthy**     | {::nomarkdown}<img src="../../../images/icons/current-state-healthy.png" display=inline-block">{:/} |                        
| **Degraded**    | {::nomarkdown}<img src="../../../images/icons/current-state-degraded.png" display=inline-block/>{:/} |
| **Suspended**   | {::nomarkdown}<img src="../../../images/icons/current-state-suspended.png" display=inline-block">{:/} |  
| **Missing**     | {::nomarkdown}<img src="../../../images/icons/current-state-missing.png" display=inline-block">{:/} |  
| **Progressing** | {::nomarkdown}<img src="../../../images/icons/current-state-progressing.png" display=inline-block">{:/} | 
| **Unknown**     | {::nomarkdown}<img src="../../../images/icons/current-state-unknown.png" display=inline-block">{:/} |  



##### Resource sync state in Tree view
Identify the sync state of an application resource through the icon on the left of the resource name and the color of the resource name. For information on sync options you can configure for applications, see [Sync settings]({{site.baseurl}}docs/deployment/create-application/#sync-settings).

{: .table .table-bordered .table-hover}
| Sync state     | Display in Tree view  |  
| -------------- | ----------        |  
| **Synced**       | {::nomarkdown}<img src="../../../images/icons/current-state-synced.png" display=inline-block">{:/} |                            
| **Syncing**      | {::nomarkdown}<img src="../../../images/icons/current-state-syncing.png" display=inline-block/>{:/} |  
| **Out-of-Sync**  | {::nomarkdown}<img src="../../../images/icons/current-state-out-of-sync.png" display=inline-block">{:/} |  
| **Unknown**      | {::nomarkdown}<img src="../../../images/icons/current-state-sync-unknown.png" display=inline-block">{:/} |  


##### Resource inventory in Tree view
The Resource inventory in the Tree view (bottom-left), summarizes the aggregated count for each resource type in the application.  
For visibility and quick access, `Syncing` and `Out-of-Sync` resources are bucketed separately. 

{::nomarkdown}
<br>
{:/}

**Click-filters**  
In the resource inventory, selecting a `Syncing` or `Out-of-Sync` resource type, filters the Current State by that resource type and sync state.
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



### View application deployments  Application timeline 
The Timeline tab displays the history of deployments for the selected application, sorted by the most recent update (default), and enriched with image, Pull Request (PR), Jira issues, and commit information for each deployment. Ongoing deployments display rollout progress and rollout analysis as the deployment unfolds.   
 
Each application displays up to ten of the most recent deployments. 

{% include
image.html
lightbox="true"
file="/images/applications/dashboard-timeline-main.png"
url="/images/applications/dashboard-timeline-main.png"
alt="Applications Dashboard: Timeline tab"
caption="Applications Dashboard: Timeline tab"
max-width="30%"
%}

#### Filters

View the subset of deployments of interest to you. Filter by Date range, PRs, issues, committers, and more.  

#### Deployment chart

The deployment chart conveniently located below the filters displays the day-to-day deployments for the selected time period. Get information on historical deployments, as the deployment entries are shown for up to ten of the most recent deployments.  

* To jump to a specific deployment, click the dot on the chart that represents the deployment. 
* To see GitOps details, mouse over the dot that represents the deployment. 

{% include
image.html
lightbox="true"
file="/images/applications/apps-historical-deployment.png"
url="/images/applications/apps-historical-deployment.png"
alt="Applications Dashboard: Deployment chart"
caption="Applications Dashboard: Deployment chart"
max-width="30%"
%}

#### Deployment details

**Deployment entries**  

Each deployment entry displays the complete history of that deployment, by Git hash, Kubernetes services, and Argo applications:


{% include
image.html
lightbox="true"
file="/images/applications/app-dashboard-time-expanded-card.png"
url="/images/applications/app-dashboard-time-expanded-card.png"
alt="Applications Dashboard: Deployment entry in Timeline tab"
caption="Applications Dashboard: Deployment entry in Timeline tab"
max-width="70%"
%}

* The **CI Builds** shows the image(s) created or updated during deployment. Click to see the **Images** view in a new browser window.
* The **Pull Request (PRs)** used for the commit.
* The Jira **Issues** the PR aims to resolve or has resolved, with the current status.
* The **Committer** who made the changes.
* The Kubernetes **Services** updated during the deployment, according to the Argo rollout strategy, and the current rollout status. 
  The example above shows the rollouts according to the canary rollout strategy. The blue line shows the progress of rollout with the current step versus the total number of steps.  
  Expanding the live version deployment shows the number of replicas currently deployed, as green circles.
* The Argo **Applications** updated during this deployment.

#### Rollout progress visualization
A live deployment, meaning an Argo CD sync due to a change in the desired state, is visualized through the progress bar, as the rollout.
> Rollout progress is displayed only if you have an Argo `rollout` resource defined for your application. 

For detailed information, see [Argo Rollouts documentation](https://argoproj.github.io/argo-rollouts/){:target="\_blank"}.

Here is an example of the rollout progress bar for an ongoing canary deployment.  The rollout comprising four steps has not started, and traffic has not been routed as yet to the new version of the application.

{% include
image.html
lightbox="true"
file="/images/applications/apps-dashboard-rollout-in-progress.png"
url="/images/applications/apps-dashboard-rollout-in-progress.png"
alt="Application deployment in progress"
caption="Application deployment in progress"
max-width="50%"
%}

Here is an example of the rollout progress bar for the same deployment on completion. All traffic has been routed to the new version. 

{% include
image.html
lightbox="true"
file="/images/applications/apps-dashboard-rollout-complete.png"
url="/images/applications/apps-dashboard-rollout-complete.png"
alt="Application deployment completed"
caption="Application deployment completed"
max-width="50%"
%}

#### Rollout steps visualization
Clicking the rollout name displays the visualization of the steps in the rollout.  

Argo defines an analysis run as an instantiation of an AnalysisTemplate. The result of an analysis run determines if the rollout is completed, paused, or aborted. For detailed information, see the [Analysis section in Argo Rollouts](https://argoproj.github.io/argo-rollouts/features/analysis/){:target="\_blank"}.  

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

To view run results, expand **Analysis metrics** in Background Analysis.   
The Manifest tab displays the analysis template manifest.

{% include
image.html
lightbox="true"
file="/images/applications/apps-dashboard-run-results.png"
url="/images/applications/apps-dashboard-run-results.png"
alt="Application rollout: Analysis run results"
caption="Applications rollout: Analysis run results"
max-width="30%"
%}


### Application services
The Services tab for an application shows the K8s services for each deployment of the application. 
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




