---
title: "Applications dashboard"
description: ""
group: deployment
toc: true
---

View, monitor, and analyze deployments across your enterprise in the Applications dashboard. As a one-stop shop for Argo Rollouts and Argo CD in Codefresh, the Applications dashboard delivers on the challenge of keeping track of deployments, whatever the frequency and scale.  

The Applications dashboard is to continuous delivery (CD) what the Workflows dashboard is to continuous integration (CI):


Populated with deployments from all the clusters associated with the provisioned runtimes, in-cluster or managed. 
Get details on curent and historical deployments
GitOps-centric with deployment details enriched with CI information: Links to container images, Git hashes correlated with feature requests, and i and  roduct value is not measured in Git hashes, but instead in the number and significance of new features that are deployed to production. New features are typically recorded in an issue management system (e.g. JIRA) and have their own lifecycle according to what the organization desires.
Application services 


\One of the biggest challenges that companies face when they start doing lots of frequent deployments (CD) is that the volume of deployments makes it very difficult to keep track of which changes have been deployment to each application/environment. Our Application dashboards make this super easy - you can search for Jira issues, commit messages, committers, etc, and see exactly when/if that change was applied to a specific Application. Also, each Application give a ton of other information about each change, such as all the microservices that were deployed as part of the change, links to their previous + new container images, links to their Git commit, etc. To sum up, the application dashboards add this awesome traceability and visibility to CD.

Apart from Git hashes, you can also filter by features such as Jira issues P

As a first step towards the vision of GitOps, we have decided to address this information gap and solve the observability problem by combining these two kinds of information (git hashes and features) in a single cohesive view.

You can now use the new Codefresh dashboard and correlate extra information on top of a single Git hash.

Monitor and track all aspects of your applications and deployments, from the source and destination targets, to an enriched deployment history, services and current status.
Gte real-time updates on deployment status
Visibility into the cluster - services and resoruces Similar to P other dashboards in CSDP, the Applications dashboard displays deployment information on two levels.
The first level is a list view of of all the applications based on their struture, with cluster, namespave and runtime infromation in addiotn to the status.
Zoom in for a detailed look at the application components, services, resoirc second level offers a detailed look at the deployment, including the timeline, services and current state. 

Details are synced at all times wit the 

Current health and sync status
Deployment graph that shows successful/failed deployments on the selected time period
Complete history of deployments according to Git hash. For each deployment you can also see which Pull Request was used for the commit, who was the committer and which JIRA issues this Pull request is solving (provided that the image was built by a Codefresh pipeline)
The Kubernetes services that belong to this application (on the services tab)
What services and replicas were updated with each deployment.
The deployment status is fetched from your ArgoCD integration in a live manner. If, at any point, the deployment is not synced with GIT, you will instantly see the out-of-sync status. You will get the number of resources that are out of sync. When you click the out-of-sync status, you will get a list of all resources in that status.

Get a igh-lvel view of llal the applications deployed across clusters . Customize the view to show only what you need with filters and favoirte tagging. Explore the application deployment model, and then drill down to indiviaul applications to see deployment timelines, services, and resources.

Immediately idemtifi the application state with the bearkdown snaphot



Sync and health status
Structure of applications for app sets and app of apps
Visibility into deployments

### Applications main view

The main view shows all deployed applications, sorted by the most recent deployment, by default. 


Here is an example of the main page in the Applications dashboard. 

  SCREENSHOT

#### Application inventory and state 

The application state snapshot shows at a glance both the total number of applications that are deployed, and their breakdown according to state.

> The state 


####  Filters and favorites
Similar to other dashboards, the Applications dashboard also offers a set of filters designed to bring you the information you need as quickly as possible.  
Marking applications as favorites are also a way to focus on those of interest

**Filters**  
There are the frequently used filters and advanced filters.
* Frequently-used filters: Available at the top of the dashboard. These filters support multi-selection, and results are based on an OR relationship within the same filter with multiple options, and an AND relationship between filters.
* Advanced filters: Available on selecting More Filters. These filters include application type, health, and labels. 
  * Application type : Applications and ApplicationSet
    
  * Health filters  
    Include the built-in Argo CD set of health filters, and custom health filters such as Missing and Unknown.  
    For Argo CD health filters, see [Health sets](https://argo-cd.readthedocs.io/en/stable/operator-manual/health/). 
    Missing:  
    Unknown:  
  
  * Labels  
    The K8s labels defined for the applications. To see the available labels, select **Add** and then select the required labels and values. Remember to select **Add** again to add the selected labels as filters.  
    For detailed information, see the official documentation on [Labels and selectors](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/){:target="\_blank"}.


**'Favorite' applications**
Mark applications as favorites to view them with a click.
* Select the {% include}<img src="../../../images/icons/icon-mark-favorite.png">{%}  to the left of the application name to mark it as a favorite. 
* To view only favorites, on the filters bar, select {% include}<img src="../../../images/icons/icon-fav-starred.png">{%}.



#### 3 Deployment models
Applications are displayed according to their deployment mode which can be one of the following:
* Applications  
  Standalone applications. For detailed information, see the official documentation on [Applications](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#applications){:target="\_blank"}.  

* Application set 
  Based on the Argo CD's ApplicationSet CRD, where several applications are always deployed as a set. For detailed information, see the official documentation on [Generating Applications with ApplicationSet](https://argo-cd.readthedocs.io/en/stable/user-guide/application-set/){:target="\_blank"}.
  
* App-of-apps
  In this deployment model, the parent application deploys a set of child applications.  For detailed information, see the official documentation on [App of Apps](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#app-of-apps){:target="\_blank"}. 


### Application timeline 
The Timeline tab displays the history of deployments for the selected application, sorted by the most recent update (default), enriched with image, PR, Jira issues, and commit information for each deployment. 


The header displays key information on the currently deployed version.

Filters
Filter deployments to view the subset of interest to you, by Date range, PRs, issues, committers and more.  match your custom criteria.

**Deployment chart**
View day-to-day deployment information for the selected time period. The deployment chart is useful to get information on historical deployments, as deployment cards are shown for up to ten of the most recent deployments.  

* To jump to a specific deployment, click the dot on the chart that represents the deployment. 
* To see GitOps details, mouse over the dot that represents the deployment. 

**Deployment card**
Each deployment card or entry displays the complete history of deployments according to Git hash:


* The **CI Builds** which are the image(s) created or updated during deployment. Click to see the **Images** view in a browser window.
* The **Pull Request (PRs)** used for the commit.
* The Jira **Issues** the PR aims to resolve or has resolved, with the current status.
* The **Committer** 
* A visualization for the Kubernetes **Services** updated during the deployment, according to the Argo rollout strategy, and the current rollout status.  
  The example above shows the rollouts according to the canary rollout strategy. The blue line shows the  progress of rollout, with the gradual transfer of traffic to the stable.
* The Argo **Applications** updated for this deployment.




### Application services
The Services tab for an application shows the services and replicas updated with each deployment.

### Application current state
The Current State tab for an application shows the live state of all the application's resources (Kubernetes objects) on the cluster, in a hierarchical view, sorted by the Last Update.
Every resource is identified by a unique icon and the label. 



You can filter by:
* **Kind**: The type of Kubernetes resource. 
* **Health**: The status of the resource.

For more information, see the official documentation on [Working with Kubernetes objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/){:target="\_blank"}.

