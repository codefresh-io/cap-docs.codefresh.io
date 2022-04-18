---
title: "What's new in CSDP?"
description: ""
group: whats-new
redirect_from:
  - /docs/whats-new/
toc: true
---


We launched the Codefresh Software Delivery Platform (CSDP) in February this year. Built on Argo, the world’s most popular and fastest-growing open source software delivery, CSDP unlocks the full enterprise potential of Argo Workflows, Argo CD, Argo Events, and Argo Rollouts, providing a control-plane for managing them at scale.
Since the launch, we have continued to work on and grow CSDP.

### Features and enhancements 

#### Kubernetes cluster server version
We now support the latest Kubernetes server versions, 1.22 and 1.23. 

#### Ingress controllers
We are continually working on supporting additional Ingress controllers, and this release includes support for four more of them:
* Ambassador
* NGINX Enterprise
* Istio
* Traefik

All ingress controllers must be configured to report their status. 
For details, see [Ingress controller requirements]({{site.baseurl}}/docs/runtime/requirements/#ingress-controller).


#### External cluster support
Argo CD can manage external clusters without Argo CD installed on them. CSDP offers the same functionality to add, view, and manage remote clusters.  
CSDP admins can add an external cluster to a CSDP runtime, and register it automatically as a managed cluster. From that point on, you have complete visibility into health and sync status, and options to manage them.  

With managed clusters in CSDP, you get:
* Streamlined management: All cluster- and cluster-component level operations are managed through the runtime, in a centralized location. You can install new and uninstall existing components, and remove the cluster from the runtime's managed list.
* Seamless upgrades: Upgrades to runtimes or to runtime components in the local cluster automatically upgrades those in managed clusters as well.
* Integration with CSDP dashboards: Applications dashboards reflect deployment information for applications in all managed clusters.

For details, see [Managed clusters]({{site.baseurl}}/docs/runtime/managed-cluster).

#### Topology views for runtimes
This release introduces the Topology view for runtimes. Get a visual representation of the runtimes in your deployments, managed clusters, and cluster components. 
Quickly identify key information such as health and sync status, and version.
Add new clusters to or remove existing clusters from runtime management.  

For details, see [Topology view for runtimes]({{site.baseurl}}/docs/runtime/monitor-manage-runtimes/#topology-view).

#### Applications dashboard enhancements
View, monitor, and analyze deployments across your enterprise in the Applications dashboard. As a one-stop shop for deployments, the Applications dashboard delivers on the challenge of keeping track of your deployments, whatever the frequency, and whatever the scale.  


Here are the main enhancements:
* Filters:  
  The health status snapshot in the Applications dashboard also works as a quick filter. Select a status to quickly filter applications by that status.    
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

* Rollouts: 
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

* Git committers  
  Selecting a PR annotation shows all Git committers for that PR.  

  {% include 
	image.html 
	lightbox="true" 
	file="/images/whats-new/wrkflow-template-main.png" 
	url="/images/whats-new/wrkflow-template-main.png" 
	alt="Workflow Templates" 
	caption="Workflow Templates"
  max-width="30%" 
  %}

* Current state of cluster resources  
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
CSDP provides full-fledged management for Workflow Template resources, from optimizing existing Workflow Templates, to creating new ones, and testing Workflow Templates before commit. 
 
 {% include 
	image.html 
	lightbox="true" 
	file="/images/whats-new/wrkflow-template-main.png" 
	url="/images/whats-new/wrkflow-template-main.png" 
	alt="Workflow Templates" 
	caption="Workflow Templates"
  max-width="30%" 
  %}

* **Create Workflow Templates**  
  Create Workflow Templates in three steps. Start by selecting one from the Codefresh Hub for Argo, or start with a blank template form. Customize. And then submit.  

  {% include 
	image.html 
	lightbox="true" 
	file="/images/whats-new/wrkflow-template-add.png" 
	url="/images/whats-new/wrkflow-template-add.png" 
	alt="Add Workflow Template panel" 
	caption="Add Workflow Template panel"
  max-width="30%" 
  %}

* **Workflow Template Manifests, Workflows and Pipelines**  
  Select a Workflow Template, and work with the options in the Manifest tab to update and optimize it. Any change to the Git State manifest is immediately displayed with the before and after change versions.  
  See the Workflows and Delivery Pipelines associated with the selected Workflow Template.  
 
  {% include 
	image.html 
	lightbox="true" 
	file="/images/whats-new/rel-notes-wrkflow-temp-manifest-run.png" 
	url="/images/whats-new/rel-notes-wrkflow-temp-manifest-run.png" 
	alt="Workflow Template: Manifest, Workflow and Pipeline tabs" 
	caption="Workflow Template: Manifest, Workflow and Pipeline tabs"
  max-width="30%" 
  %}

* **Workflow Templates sandbox**  
  Test your changes to any Workflow Template  without having to first commit the changes.  
  Simply run the Workflow Template. Scroll through previous iterations if any, view arguments and values, and change, add, or delete them. 
  
  {% include 
	image.html 
	lightbox="true" 
	file="/images/whats-new/rel-notes-wrkflow-temp-run-args-view.png" 
	url="/images/whats-new/rel-notes-wrkflow-temp-run-args-view.png" 
	alt="Run Workflow Template: Arguments list" 
	caption="Run Workflow Template: Arguments list"
  max-width="30%" 
  %}


#### Application creation wizard
Create applications from the Applications dashboard. Define the application name and the runtime to use. CSDP automatically creates the YAML resource with the same name as the application.  
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
Our Delivery Pipeline flows have several usability and functionality enhancements.
* Git repo selection  
  A dropdown list allows you to select one or more Git repos in the Trigger Conditions tab. Start typing, and use autocomplete to view and select from the available Git repos.

* Commits
Commits are blocked for users without write permissions.
Users who work on an older revision because of merge conflicts in the previous revision, now have the option of committing their changes. 


#### New status for active workflows without events
Identify workflows that are active but do not have any execution data with the new status filter in the Workflows dashboard. Filtering by Status ‘Unknown’ shows workflows without events for the last hour.


#### Docker config.json to report image info
You can now authenticate to a Docker registry using the docker./config. json. Note that  config.json is not currently supported for GCR, ECR, and ACR.


#### OpenShift 4.8 support 
CSDP supports Red Hat OpenShift 4.8. For detailed information, read their [blog](https://cloud.redhat.com/blog/red-hat-openshift-4.8-is-now-generally-available#:~:text=OpenShift%204.8%20improves%20the%20bare,is%20now%20shipping%20with%20OpenShift){:target="\_blank"}.

### Bug fixes
* Inaccurate results when filtering by Application type.
* Fixed issue when new agent upgrade was overriding some configuration in previous release.
* JIRA annotations are not displayed in Docker.io images.
* Cluster shows information for ArgoCD cluster instead of for the target cluster.
* Avatars show up intermittently.
* Uninstallation issues with newer K8s versions.
* Creating a new pipeline with an existing Template shows empty Template tab.
* Incorrect Committers in Applications dashboard.
* Applications dashboard performance issues.
