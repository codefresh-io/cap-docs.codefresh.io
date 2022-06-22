---
title: "Hosted GitOps"
description: ""
group: incubation
toc: true
---


      



Codefresh has enhanced solution offering with Hosted GitOps, the SaaS version of Codefresh.  

Achieve your CD Ops goals with Hosted GitOps: easy set up and zero maintenance overhead, combined with advanced functionality for progressive delivery and deployments.

### What do you get with Hosted GitOps?

**Hosted runtimes**  

Setting up your hosted environment takes a few clicks. All you need is a Codefresh account, a Git account, and a Kubernetes cluster to which to deploy your applications.
Codefresh guides you through the simple three-step process of provisioning your hosted runtime.  

SCREENSHOT   

From version updates to security updates, Codefresh handles the administration and maintenance of hosted runtimes. 

**Advanced CD Ops**  

From application analytics, to application creation, rollout and deployment, you get the best of both worlds: Argo CD and Argo Rollouts with our advanced CD functionality.

* Application analytics  

  Highlights and comprehensive analytics through a series of dashboards: DORA metrics, Home, and Applications.
  * DevOps performance through DORA metrics
  * Daily deployments, failed deployments with rollbacks, and most active applications in the Home analytics dashboard
  * Deep dive into deployment in the Applications dashboard

SCREENSHOT 1 of DORA
SCREENSHOT 2 of Home

* Application creation  

  Create applications in a single location. Define application settings through the intuitive Form mode, or direclty in YAML. Commit all changes to Git.

* Application rollout  

  Track progress as the rollout occurs, visualize every step in the rollout as they occur, from the Applications dashboard. 

* Application deployment  

  Historical deployments, current deployments, current state of application resources in interactive tree view with search functionality.  

**CI connects for image enrichment**  

Connect your CI integration tools to Codefresh to enrich deployment information.

* Git PRs (Pull Requests), Commits, and Committer information directly from the Code repository
* Jira ticket information for correlation with deployed features  
* Container registry integration with DockerHub and Quay to fetch image information 

{% include 
	image.html 
	lightbox="true" 
	file="/images/incubation/github-action-int-settings.png" 
	url="/images/incubation/github-action-int-settings.png" 
	alt="Image enrichment with GitHub Actions integration" 
	caption="Image enrichment with GitHub Actions integration"
    max-width="30%" 
%}

### Hosted vs. hybrid comparison

{: .table .table-bordered .table-hover}
| Feature                 |Functionality  |  Hosted                | Hybrid | 
| --------------          | --------------|--------------- | --------------- |
| Runtime                 | Installation       | Managed by Codefresh   | Managed by customer       | 
|                         | Number per account | Only one runtime        | Multiple runtimes  | 
|                         | Management         | Only uninstall          | All options available: Upgrade, installation, uninstall   | 
| Clusters                 |In-cluster          | Managed by Codefresh    | Managed by customer       | 
|                          |Managed             | Managed by customer       | Mamaged by customer         | 
| CI Ops                  | Delivery Pipelines |Not supported            | Supported  | 
|                         |Workflows           | Not supported           | Supported  | 
|                         |Workflow Templates  | Not supported           | Supported  | 
| CD  Ops                 |Applications        | Fully  supported        |  Fully supported | 
|                         | Image enrichment    | Supported              | Supported  | 
|                         | Rollouts            | Supported              |  Supported  | 
|Integrations             |                     | Supported              | Supported  | 
|Dashboards               |(Main) Analytics      | Available with analytics for hosted runtime and deployments| Available with analytics for runtimes, deployments, Delivery Pipelines | 
|                         |DORA metrics         | Available |Available        | 
|                         |Applications         | Available |Available        | 

