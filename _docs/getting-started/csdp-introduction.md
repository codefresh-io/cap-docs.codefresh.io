---
title: "Introducing Codefresh"
description: ""
group: getting-started
toc: true
---

Codefresh is a full-featured, turn-key solution for application deployments and releases. Powered by the Argo Project, Codefresh uses Argo CD, Argo Workflows, Argo Events, and Argo Rollouts, extended with unique functionality and features essential for enterprise deployments.  

Codefresh offers security, maintainability, traceability, and most importantly, a single control plane for all stakeholders, be they developers, operators, product owners or project managers.
 
With Codefresh teams can:
 
* Deliver software at scale by managing hundreds or thousands of deployment targets and applications
* Get a secure, enterprise-ready distribution of Argo with built-in identity, RBAC (role-based access control), and secrets
* Gain clear visibility across all deployments and trace changes and regressions from code to cloud in seconds
* Get enterprise-level dedicated support for Argo deployments
 
### Codefresh offerings

Codefresh has two solution offerings:  

* **Hosted GitOps**, a hosted and managed version of Argo CD. The SaaS version of Codefresh, the runtime is hosted on a Codefresh cluster (easy setup) and managed by Codefresh (zero maintenance overhead).  
From application analytics, to application creation, rollout, and deployment, you get the best of both worlds: the power of Argo CD and unique features and functionality from Codefresh to help achieve your CD goals.  

* **Hybrid GitOps**, with the runtime hosted on the customer cluster and managed by the customer. The hybrid offering combines the power of Argo CD with Codefresh's CI and CD arsenal, to help achieve your continuous integration and continuous deployment and delivery goals.

### Codefresh and open source Argo
Codefresh brings the power of the Argo project to your Kubernetes deployments:  

* Argo CD for declarative continuous deployment 
* Argo Rollouts for progressive delivery 
* Argo Workflows as the workflow engine 
* Argo Events for event-driven workflow automation framework

Codefresh creates a conformed fork of the Argo project, providing an enterprise-supported version of the same, enhanced with unique functionality.
For details, see [Codefresh architecture]({{site.baseurl}}/docs/getting-started/architecture).

 
### Codefresh and GitOps
Codefresh is GitOps-centric, and supports GitOps from the ground up. Codefresh leverages Argo components to have the entire desired state applied from Git to your Kubernetes cluster, and then reported back to Codefresh.  In addition:  

* Every state change operation in Codefresh is made via Git  
* The Codefresh audit log is derived from the Git changelog  
* Codefresh access control is derived from Git permissions  

For details, see [entity model]({{site.baseurl}}/docs/getting-started/entity-model) and [access control]({{site.baseurl}}/docs/administration/access-control).
 

### Codefresh and insights
Codefresh makes it easy to both access and visualize critical information for pipelines, workflows, and deployments at any level, and for anyone, from managers to DevOps engineers. 

#### Global deployment analytics  

The Home dashboard presents enterprise-wide deployment highlights, making it a useful management tool.  
Get insights into important KPIs and deployments, across runtimes and clusters, all in the same location. View status of runtimes and managed clusters, deployments, failed deployments with rollbacks, most active applications, and Delivery Pipelines.  

{% include
 image.html
 lightbox="true"
 file="/images/incubation/home-dashboard.png"
 url="/images/incubation/home-dashboard.png"
 alt="Global deployment analytics"
 caption="Global deployment analytics"
    max-width="80%"
%}

#### DORA metrics

DORA metrics has become integral to enterprises wanting to quantify DevOps performance, and Codefresh has out-of-the-box support for it.

Apart from the metrics themselves, the DORA dashboard in Codefresh has several features such as the Totals bar with key metrics, filters that allow you to pinpoint just which applications or runtimes are contributing to problematic metrics, and the ability to set a different view granularity for each DORA metric.  

See [DORA metrics]({{site.baseurl}}/docs/reporting/dora-metrics/).

{% include
 image.html
 lightbox="true"
 file="/images/incubation/intro-dora-metrics.png"
 url="/images/incubation/intro-dora-metrics.png"
 alt="DORA metrics"
 caption="DORA metrics"
    max-width="60%"
%}
#### Application analytics and analysis

The Applications dashboard displays applications across runtimes and clusters, from which you can select individual applications for analysis. No matter what the volume and frequency of deployments, our Applications dashboard makes it super easy to track them. Search for Jira issues, commit messages, committers, and see exactly when and if the change was applied to a specific application. 

See [Applications dashboard]({{site.baseurl}}/docs/deployment/applications-dashboard/).

{% include
 image.html
 lightbox="true"
 file="/images/applications/app-dashboard-main-view.png"
 url="/images/applications/app-dashboard-main-view.png"
 alt="Applications dashboard"
 caption="Applications dashboard"
    max-width="80%"
%}

#### Delivery Pipelines
View analytics for a Delivery Pipeline and its workflows 

### Codefresh and CI/CD


#### Application  management

Manage the entire application development lifecyle in the Codefresh UI, from creating the application, to editing and deleting them.  

Define all application settings in a single location through the intuitive Form mode or directly in YAML, and commit all changes to Git.  
For easy access, the configuration settings are available in the Applications dashboard along with the deployment and resource information.

See [Applications]({{site.baseurl}}/docs/deployment/create-application/).

{% include
 image.html
 lightbox="true"
 file="/images/applications/add-app-general-settings.png"
 url="/images/applications/add-app-general-settings.png"
 alt="Application creation in Codefresh"
 caption="Application creation in Codefresh"
    max-width="60%"
%}

#### Delivery Pipelines

Delivery Pipelines are where all the magic happens in Codefresh. Our pipeline creation wizard removes the complexity from creating, validating, and maintaining pipelines. Every stage has multi-layered views of all the related Git change information for the pipeline.  

#### Workflows 
Drill down into a workflow to visualize the connections between the steps in the workflow.
A unique feature is the incorporation of Argo Events into the workflow visualization. You get a unified view of Argo Events and Argo Workflows in one and the same location, the events that triggered the workflow combined with the workflow itself.

#### Workflow Templates
Codefresh provides full-fledged management for the Workflow Template resource, from optimizing existing Workflow Templates, to creating new ones, and testing Workflow Templates before commit.  
Select a Workflow Template from Codefresh Hub for Argo, or start with a blank template form. The **Run** option sugnificantly enhances usability. You can test a new template, or test changes to an existing template, without needing to first commit the changes.  
 
 {% include 
	image.html 
	lightbox="true" 
	file="/images/whats-new/wrkflow-template-main.png" 
	url="/images/whats-new/wrkflow-template-main.png" 
	alt="Workflow Templates" 
	caption="Workflow Templates"
  max-width="70%" 
  %}

### Third-party CI integrations

If you have Hosted GitOps, you can still enrich your deplyments with CI information while retaining existing CI processes. Simply connect your CI tools for pipelines and workflows to Codefresh, and our new report image template brings in the infromation.  For example, add the report image step in your GitHub Actions pipeline and reference the different integrations for Codefresh to retrieve and enrich the image with Jira ticket information.  

See [Image enrichment with integrations]({{site.baseurl}}/docs/integrations/image-enrichment-overview/).

{% include
 image.html
 lightbox="true"
 file="/images/incubation/github-action-int-settings.png"
 url="/images/incubation/github-action-int-settings.png"
 alt="Image enrichment with GitHub Actions integration"
 caption="Image enrichment with GitHub Actions integration"
    max-width="60%"
%}

### Codefresh and continuous integration




### Codefresh user interface
And finally, the Codefresh UI gives you easy access to all the functionality, and visibility at all times to key information:  

* Runtimes management  
  View and manage all the runtimes in your deployment in the Runtimes dashboard. Get notified when versions are updated, view the changelog, and then decide if to upgrade. Detect health and sync errors at a glance in the Sync Status column. At any point, drill down into a runtime for detailed information on its components.
* Applications dashboards for CD tracking  

* 

### What to read next
[Quick start tutorials]({{site.baseurl}}/docs/getting-started/quick-start)