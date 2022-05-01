---
title: "What's new in Codefresh?"
description: ""
group: whats-new
redirect_from:
  - /docs/whats-new/
toc: true
---


We launched the Codefresh platform in February this year. Built on Argo, the world’s most popular and fastest-growing open source software delivery, Codefresh unlocks the full enterprise potential of Argo Workflows, Argo CD, Argo Events, and Argo Rollouts, providing a control-plane for managing them at scale.
Since the launch, we have continued to work on and grow Codefresh. 

### Features and enhancements 

#### Kubernetes version runtime support
We now support the latest Kubernetes server versions, 1.22 and 1.23. 

#### Ingress controllers
We are continually working on supporting additional Ingress controllers, and this release adds support for:
* Ambassador
* NGINX Enterprise
* Istio
* Traefik

All ingress controllers must be configured to report their status. 
For details, see [Ingress controller requirements]({{site.baseurl}}/docs/runtime/requirements/#ingress-controller).


#### Argo CD managed cluster support
Argo CD can manage clusters without Argo CD installed on them. Now you have the same functionality in Codefresh, and can add, view, and manage remote clusters.  
Admins can add an external cluster to a Codefresh runtime, and register it automatically as a managed cluster. From that point on, you have complete visibility into health and sync status, and options to manage them, including installing Argo Rollouts.  

With managed clusters in Codefresh, you get:
* Streamlined management: All cluster- and cluster-component level operations are managed through the runtime, in a centralized location. You can install new and uninstall existing components, and remove the cluster from the runtime's managed list.
* Seamless upgrades: Upgrades to runtimes or to runtime components in the local cluster automatically upgrades those in managed clusters as well.
* Integration with dashboards: Applications dashboards reflect deployment information for applications in all managed clusters. Argo CD Rollouts are installed, application rollouts are also reported to the dashboard. 

For details, see [Managed clusters]({{site.baseurl}}/docs/runtime/managed-cluster).

#### Topology views for runtimes
Get a visual representation of the runtimes in your deployments, managed clusters, and cluster components with the Topology view for runtimes. 
Quickly identify key information such as health and sync status, and version.
Add new clusters to or remove existing clusters from runtime management.  

For details, see [Topology view for runtimes]({{site.baseurl}}/docs/runtime/monitor-manage-runtimes/#topology-view).

#### Analytics dashboard
In addition to Delivery Pipelines, the Analytics dashboard shows Runtimes, Managed Clusters, Deployments, and Applications, to give you the complete CI/CD picture with key facts and insights.  

**Usability enhancements**  
  * Global filters are now located at the top of the dashboard.  
  * Resource-specific filters are available for that resource. 
  * A convenient View button takes you to the dedicated resource view for additional analysis.


{% include 
	image.html 
	lightbox="true" 
	file="/images/whats-new/rel-notes-analytics-dashboard.png" 
	url="/images/whats-new/rel-notes-analytics-dashboard.png" 
	alt="Analytics dashboard" 
	caption="Analytics dashboard"
   max-width="30%" 
  %}




#### Applications dashboard enhancements
The Applications dashboard displays the individual deployments across your enterprise.  Here are the main enhancements:  

**Application inventory and status filters**  
  
  The health status snapshot in the Applications dashboard also works as a quick filter. Selecting a status, filters applications by that status.    
  Filter criteria that match child applications automatically expands the parent application to show the child applications.

   {% include 
	image.html 
	lightbox="true" 
	file="/images/whats-new/app-dashboard-status-filter.png" 
	url="/images/whats-new/app-dashboard-status-filter.png" 
	alt="Applications Dashboard: Filter by status" 
	caption="Applications Dashboard: Filter by status"
  max-width="30%" 
  %}

**Rollouts**  

  Intuitive visualization with the option to open the Images view in a new browser window.  

   {% include 
	image.html 
	lightbox="true" 
	file="/images/whats-new/rel-notes-apps-open-image.png" 
	url="/images/whats-new/rel-notes-apps-open-image.png" 
	alt="Applications dashboard: Link to Image view" 
	caption="Applications dashboard: Link to Image view"
  max-width="30%" 
  %}

**Git committers**  
  Selecting an avatar shows all commits made by that committer.  


**Current state of cluster resources**  
  Hierarchical representation of the resources in the cluster where the application is deployed in the Current State.

  {% include 
	image.html 
	lightbox="true" 
	file="/images/whats-new/rel-notes-app-current-state.png" 
	url="/images/whats-new/rel-notes-app-current-state.png" 
	alt="Applications dashboard: Current State" 
	caption="Applications dashboard: Current State"
  max-width="30%" 
  %}

#### Workflow Templates
Codefresh provides full-fledged management for the Workflow Template resource, from optimizing existing Workflow Templates, to creating new ones, and testing Workflow Templates before commit. 
 
 {% include 
	image.html 
	lightbox="true" 
	file="/images/whats-new/wrkflow-template-main.png" 
	url="/images/whats-new/wrkflow-template-main.png" 
	alt="Workflow Templates" 
	caption="Workflow Templates"
  max-width="30%" 
  %}

**Create, test, and optimize Workflow Templates**  
  Create Workflow Templates in three steps. Start by selecting one from the Codefresh Hub for Argo, or start with a blank template form. Customize the Workflow Template. And then either run to test the template or submit it.  

  {% include 
	image.html 
	lightbox="true" 
	file="/images/whats-new/wrkflow-template-add.png" 
	url="/images/whats-new/wrkflow-template-add.png" 
	alt="Add Workflow Template panel" 
	caption="Add Workflow Template panel"
  max-width="30%" 
  %}

  For both new and existing Workflow Templates, the **Run** option enables you to test new templates, and changes to existing ones without having to first commit the changes. If the Workflow Template has previous iterations, you can scroll through them, view and change arguments and values. 

    {% include 
	image.html 
	lightbox="true" 
	file="/images/whats-new/rel-notes-wrkflow-temp-manifest-run.png" 
	url="/images/whats-new/rel-notes-wrkflow-temp-manifest-run.png" 
	alt="Workflow Template: Manifest, Workflow and Pipeline tabs" 
	caption="Workflow Template: Manifest, Workflow and Pipeline tabs"
  max-width="30%" 
  %}

  {% include 
	image.html 
	lightbox="true" 
	file="/images/whats-new/rel-notes-wrkflow-temp-run-args-view.png" 
	url="/images/whats-new/rel-notes-wrkflow-temp-run-args-view.png" 
	alt="Run Workflow Template: Arguments list" 
	caption="Run Workflow Template: Arguments list"
  max-width="30%" 
  %}
 
  The Workflows and Delivery Pipelines tabs associated with the selected Workflow Template are displayed in the respective tabs, giving you all the information in the same location.  


**Rename Workflow Template**  
  After creating a Workflow Template, you can rename it by selecting the template and clicking **Rename**.  
  The new name must be unique within the cluster. 

  {% include 
	image.html 
	lightbox="true" 
	file="/images/whats-new/rel-notes-rename-workflow-template.png" 
	url="/images/whats-new/rel-notes-rename-workflow-template.png" 
	alt="Rename Workflow Template" 
	caption="Rename Workflow Template"
  max-width="30%" 
  %}
  

#### Application creation wizard
Create applications from the Applications dashboard. Define the application name and the runtime to use. The platform automatically creates the YAML resource with the same name as the application.  
Toggle between Form and YAML views as you define additional settings for the application. 

{% include 
	image.html 
	lightbox="true" 
	file="/images/whats-new/rel-notes-app-create-settings.png" 
	url="/images/whats-new/rel-notes-app-create-settings.png" 
	alt="Application settings in application creation wizard" 
	caption="Application settings in application creation wizard"
  max-width="30%" 
%}


#### Delivery Pipeline flows 
The Delivery Pipeline flow features several usability and functionality enhancements.

**Event payload, manifests, metadata for workflow steps**  

  Once a workflow is submitted for a Delivery Pipeline, the Workflows tab visualizes the connections between the steps in the workflow.  
  A significant functionality enhancement is the capability to display data for each step in the workflow. 
  Selecting a step displays a pull-out panel with the data compatible to the step type, which can be the event payload itself, manifests, or metadata.  
  Easily copy paths for attributes from event payloads, view logs, and download artifacts.  

  This example shows the panel with the event payload for the workflow.  
  
{% include 
	image.html 
	lightbox="true" 
	file="/images/whats-new/rel-notes-event-payload.png" 
	url="/images/whats-new/rel-notes-event-payload.png" 
	alt="Panel with Event Payload in Workflows tab" 
	caption="Panel with Event Payload in Workflows tab"
  max-width="30%" 
%}

  Another example shows the panel with the sensor manifest.  

{% include 
	image.html 
	lightbox="true" 
	file="/images/whats-new/rel-notes-event-srce-manifest.png" 
	url="/images/whats-new/rel-notes-event-srce-manifest.png" 
	alt="Panel with Sensor manifest in Workflows tab" 
	caption="Panel with Sensor manifest in Workflows tab"
  max-width="30%" 
%}

**Rename trigger resource** 

  Similar to Workflow Templates, you can now change the trigger name of a Delivery Pipeline. The trigger name must be unique across all the sensors associated with that pipeline. The sensor name cannot be changed. 

{% include 
	image.html 
	lightbox="true" 
	file="/images/whats-new/rel-notes-rename-pipeline-trigger.png" 
	url="/images/whats-new/rel-notes-rename-pipeline-trigger.png" 
	alt="Rename trigger option for Delivery Pipeline" 
	caption="Rename trigger option for Delivery Pipelines"
  max-width="30%" 
%}

**Git repo selection for commits**  

  A dropdown list allows you to select one or more Git repos in the Trigger Conditions tab. Start typing, and use autocomplete to view and select from the available Git repos.

{% include 
   image.html 
   lightbox="true" 
	file="/images/whats-new/rel-notes-git-repo-select.png" 
	url="/images/whats-new/rel-notes-git-repo-select.png" 
	alt="Git repo selection for Delivery Pipelines" 
	caption="Git repo selection for Delivery Pipelines"
  max-width="30%" 
%}


**Errors/warning in manifests synced with the line number in manifest**
  Clicking the line number next to an error or a warning changes focus to the line in the manifest file with the error or warning.


#### Workflows dashboard enhancements

**Link from workflows to their pipelines**  

  Workflow names in the dashboard are clickable links. Clicking a workflow name takes you directly to the pipeline associated with that workflow.

**New status for active workflows without events**  

Identify workflows that are active but do not have any execution data with the new status filter in the Workflows dashboard. Filtering by Status ‘Unknown’ shows workflows without events for the last hour.

{% include 
	image.html 
	lightbox="true" 
	file="/images/whats-new/rel-notes-workflow-unknown-status.png" 
	url="/images/whats-new/rel-notes-workflow-unknown-status.png" 
	alt="Unknown status filter for workflows" 
	caption="Unknown status filter for workflows"
  max-width="30%" 
%}

#### Docker config.json to report image info
You can now authenticate to a Docker registry using the docker./config. json. Note that  config.json is not currently supported for GCR, ECR, and ACR.


#### OpenShift 4.8 support 
CSDP supports Red Hat OpenShift 4.8. For detailed information, read their [blog](https://cloud.redhat.com/blog/red-hat-openshift-4.8-is-now-generally-available#:~:text=OpenShift%204.8%20improves%20the%20bare,is%20now%20shipping%20with%20OpenShift){:target="\_blank"}.

### Bug fixes

Applications dashboard
* Inaccurate results when filtering by Application type.
* Cluster shows the address of the ArgoCD cluster instead of the target cluster.
* (added) Broken Commit link in Application Preview
* (added) Filter by favorites does not ApplicationSets
* (added) Releases not ordered correctly
* (added) Missing Application/AppllicationSet tags 
* (added) Loop created on changing date in the Applications dashboard. (CR-8763)
* (added) Sort by type does not work  (CR-9131)
* (added) Order of rollouts in Deployment chart not aligned with order of rollouts
* (added) Missing current release label 
* (added) Missing commit message
* JIRA annotations not displayed for Images in Docker.io.
* Avatars show up intermittently.
* Incorrect Committers in Applications dashboard.
* Applications dashboard performance issues.


Fix upsert previous release (CR-9069)

Images
* (added) Duplicates of application in Images repositories with different tags
* Currently deployed image mark are unkmarked 

Pipelines
* (added) Removed empty event-sources
* (added) trim params and filters values
 (added) restrict empty trigger conditions
* (added) Missing : created/updated/deleted status for resources
* (added) Delete hook on finish to the event-source
* (added) Fixed event mapping 
* Creating a new pipeline with an existing Template shows empty Template tab.


Uninstall
* Database artifacts remain after uninstalling with `--force` flag.
* Agent upgrade overrides some configuration in previous release.
* Uninstallation issues with newer K8s versions.

