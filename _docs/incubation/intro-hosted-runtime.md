---
title: "Hosted GitOps"
description: ""
group: incubation
toc: true
---


Codefresh has enhanced our solution offering with Hosted GitOps, the SaaS version of Codefresh.  

Achieve your CD Ops goals with Hosted GitOps: easy set up and zero maintenance overhead, combined with advanced functionality for progressive delivery and deployments.

### What do you get with Hosted GitOps?

Hosted GitOps gives you a hosted and managed version of Argo CD. 

**Hosted runtimes**  

Setting up your hosted environment takes a few clicks. All you need is a Codefresh account, a Git account, and a Kubernetes cluster to which to deploy your applications.
Codefresh guides you through the simple three-step process of provisioning your hosted runtime.  

{% include 
	image.html 
	lightbox="true" 
	file="/images/incubation/intro-hosted-hosted-initial-view.png" 
	url="/images/incubation/intro-hosted-hosted-initial-view.png" 
	alt="Hosted runtime setup" 
	caption="Hosted runtime setup"
    max-width="50%" 
%}   

From version updates to security updates, Codefresh handles the administration and maintenance of hosted runtimes. 

**Advanced CD Ops**  

From application analytics, to application creation, rollout, and deployment, you get the best of both worlds: Argo CD and Argo Rollouts coupled with advanced CD functionality from Codefresh.

* Application analytics  

  Highlights and comprehensive analytics through a series of dashboards: DORA metrics, Home, and Applications.
  * DevOps performance through DORA metrics
  * Deployment analytics at your fingertips in Home analytics dashboard: Daily deployments, failed deployments with rollbacks, and most active applications
  * Deep-dive into every deployment in the Applications dashboard: Centralized cross-runtime and cross-cluster view

{% include 
	image.html 
	lightbox="true" 
	file="/images/incubation/intro-dora-metrics.png" 
	url="/images/incubation/intro-dora-metrics.png" 
	alt="DORA metrics" 
	caption="DORA metrics"
    max-width="50%" 
%}


* Application creation  

  Create GitOps-compatible applications in a single location. Define application settings through the intuitive Form mode, or directly in YAML. Commit all changes to Git.

* Application rollout  

  Track progress of the rollout, visualize every step in the rollout as it occurs, from the Applications dashboard. 

* Application deployment  

  Historical deployments, current deployments, current state of application resources in interactive tree view with search functionality.  

**Connect third-party CI to Codefresh**  

Connect your CI integration tools to Codefresh, and enrich deployment information.

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

### Hosted vs. hybrid environments
The table below highlights the main differences between hosted and hybrid environments.

{: .table .table-bordered .table-hover}
| Functionality           |Feature        |  Hosted                | Hybrid | 
| --------------          | --------------|--------------- | --------------- |
| Runtime                 | Installation       | Provisioned by Codefresh   | Provisioned by customer       | 
|                         | Number per account | Only one runtime           | Multiple runtimes            | 
|                         | Upgrade            | Performed by Codefresh     | Performed by customer | 
| Runtime cluster         |In-cluster          | Managed by Codefresh       | Managed by customer       | 
| External cluster        |                    | Managed by customer        | Managed by customer         |
| CI Ops                  | Delivery Pipelines |Not supported               | Supported  | 
|                         |Workflows           | Not supported              | Supported  | 
|                         |Workflow Templates  | Not supported              | Supported  | 
| CD  Ops                 |Applications        | Supported                  | Supported | 
|                         | Image enrichment   | Supported                  | Supported  | 
|                         | Rollouts           | Supported                  |  Supported  | 
|Integrations             |                    | Supported                  | Supported  | 
|Dashboards               |(Main) Analytics    | Available for hosted runtime and deployments| Available for runtimes, deployments, Delivery Pipelines | 
|                         |DORA metrics         | Available                 |Available        | 
|                         |Applications         | Available                 |Available        | 

### What to read next
[Hybrid runtime installation]({{site.baseurl}}/docs/runtime/installation/)  
[Provision hosted runtime]({{site.baseurl}}/docs/incubation/hosted-runtime/)