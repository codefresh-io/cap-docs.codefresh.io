---
title: "Applications dashboard"
description: ""
group: deployment
toc: true
---

View, monitor, and analyze deployments across your enterprise in the Applications dashboard. As a one-stop shop for deployments, the Applications dashboard delivers on the challenge of keeping track of your depolyments, howevery frequent and whatever the scale.  The Applications dashboard is to continuous delivery what the Workflows dashboard is to continuous integration.  

Strting with a zoom-out view of all the applications, their deployment model, and j=eky GitOps , contrinue with enriched infroma
The dashboard is populated with deployments on all the clusters associated with the provisioned runtimes, in-cluster or managed. From high-level deployment info Being GitOps-centric, the dasoard presents an enriched view, where Git hashes are correlated with feature requests and  roduct value is not measured in Git hashes, but instead in the number and significance of new features that are deployed to production. New features are typically recorded in an issue management system (e.g. JIRA) and have their own lifecycle according to what the organization desires.


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

The main view shows all deployed applications, sorted by ???. 


Here is an example of the main page in the Applications dashboard. 

  SCREENSHOT


The application state snapshot shows at a glance the breakdown according to state.
To focus on the applications/deployments of interest  


####  Filters and favorites
Similar to other dashboards in CSDP, the Applications dashboard also offers a set of filters designed to bring you the information you need as quickly as possible.  
Marking applications as favorites are also a way to quickly bring them for view.

**Filters**  
* All filters support multi-selection. Results are based on an OR relationship within the same filter with multiple options, and an AND relationship between filters.
* More Filters: Additional filters, for example, application type. 


**'Favorite' applications**
Mark applications as favorites, and then view only those with a click.
* Select the ?? to mark it as a favorite
* To view only favorites the application to the list of favorities. To view only favorites, 
. 
For the 

When you drill down into detailed deployment 
#### 2 CD snapshot
Breakdown of the applications according to state. 




#### 3 Deployment models
Applications are displayed according to their deployment model:  A group of Kubernetes resources as defined by a manifest. This is a Custom Resource Definition (CRD)
* Applications  
  Standalone applications.    

* Application set 
  Prefixed with an arrow head, it is based on the ApplicationSet CRD, where the one application deploys a set of applications.
  
* App-of-apps
  Similar to the application set, the parent application is prefixed with an arrowhead. Expanding it shows the list of child applications.  

Unlike an application set, for an app-of-apps, both the parent and the child applications are labelled as Application.  

SCREENSHOT of app of apps expanded








### Deployment 
The timeline of deployment for this feature and if the deployment was successful or not
The pipeline(s) that were used for building the container image and deploying it
The Kubernetes images that were affected
The Kubernetes services/deployments that were affected

Deployment graph that shows successful/failed deployments on the selected time period

Complete history of deployments according to Git hash. Each deployment entry shows which Pull Request was used for the commit, who was the committer and which JIRA issues this Pull request is solving (provided that the image was built by a Codefresh pipeline)

The Kubernetes services that belong to this application (on the services tab)
What services and replicas were updated with each deployment.




#### Application deployment timeline
Track the complete history of deployments for the application, by deployment timeline, by PRs, Jira issues or committers. 

Deployment graph: Shows the successful/failed deployments for the selected time range.

Deployment filters: View only the subset of deployments that match your filter criteria.
For example, filter by Pull Requests, 

Apart from direct text search, the text field also supports a simple query language with the following keywords:



#### Application services 

#### Application current state
The current state is the live state of all the application resources on the cluster, represented hierarchically.  

You can filter by:
* Kind: The type of Kubernetes resource 
* Health: The status of the resource.

