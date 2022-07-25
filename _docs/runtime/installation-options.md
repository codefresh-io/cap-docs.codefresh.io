---
title: "Installation environments"
description: ""
group: runtime
toc: true
---

Codefresh supports two installation environments:


* **Hosted** environments (Beta), with Argo CD installed in the Codefresh cluster.
  The runtime is installed and provisioned in a Codefresh cluster, and managed by Codefresh.  
  Hosted enviroments are full-cloud environments, where all updates and improvements are managed by Codefresh, with zero-maintenance overhead for you as the customer. Currently, you can add one hosted runtime per account.

  
{% include
 image.html
 lightbox="true"
 file="/images/runtime/intro-hosted-hosted-initial-view.png"
 url="/images/runtime/intro-hosted-hosted-initial-view.png"
 alt="Hosted runtime setup"
 caption="Hosted runtime setup"
    max-width="80%"
%} 

  For more information on how to set up the hosted environment, including provisioning hosted runtimes, see [Set up a hosted (Hosted GitOps) environment]({{site.baseurl}}/docs/runtime/hosted-runtime/).  

* **Hybrid** environments, with Argo CD installed in the customer's cluster.    
  The runtime is installed in the customer's cluster, and managed by the customer.  
  Hybrid environments are ideal for organizations that want their source code within their premises, or have other security constraints. Hybrid installations strike the perfect balance between security, flexibility, and ease of use. As the customer, you are responsible for installing and upgrading runtimes, while Codefresh continues to maintain other aspects of the platform.  
 
{% include
   image.html
   lightbox="true"
   file="/images/runtime/runtime-list-view.png"
 url="/images/runtime/runtime-list-view.png"
  alt="Runtime List View"
  caption="Runtime List View"
  max-width="70%"
%}

  For more information on hybrid environments, see [Hybrid runtime requirements]({{site.baseurl}}/docs/runtime/requirements/) and [Installling hybrid runtimes]({{site.baseurl}}/docs/runtime/installation/).  

### Hosted and hybrid runtime architecture
Hosted and hybrid runtimes share similar architectures, with the key difference being the location of the Codefresh Runtime.
For general architecture and component descriptions, see ???
This section provides a more detailed look into the architectures of the runtime environments.


#### Hosted runtime architecture
With Hosted GitOps, the Codefresh Runtime is located on a K8s cluster managed by Codefresh. 

{% include
   image.html
   lightbox="true"
   file="/images/runtime/arch-hosted.png"
 url="/images/runtime/arch-hosted.png"
  alt="Hosted runtime architecture"
  caption="Hosted runtime architecture"
  max-width="100%"
%}

#### Hybrid runtime architecture
{% include
   image.html
   lightbox="true"
   file="/images/runtime/arch-hosted.png"
 url="/images/runtime/arch-hosted.png"
  alt="Hosted runtime architecture"
  caption="Hosted runtime architecture"
  max-width="100%"
%}

#### Managed clusters
Managed clusters are external clusters that you register to a provisioned hosted or hybrid runtime. 
* Hosted runtime: Requires you to connect to an external K8s cluster as part of setting up the Hosted GitOps environment. You can add more managed clusters after completing the setup.
* Hybrid runtimes: You can add external clusters after provisioning hybrid runtimes.



#### Git provider repos
Codefresh Runtime creates three repositories in your organization's Git provider account:

* Codefresh runtime installation repository
* Codefresh Git Sources
* Codefresh shared configuration repository. It does so for two types of entities:

**Codefresh Runtime finctionality**
The runtime:
* Ensures that the installation repository and the Git Sources are always in sync, and applies Git changes back to the cluster
* Receives events and information from the user's organization systems to execute workflows
   By default, the ingress controller directs all requests and events to the Codefresh Application Proxy. When internal and an external ingress hosts are configured, the ingress comtroller directs webhook events to the relevant Event Source and then to Argo Events (not via the Codefresh Application Proxy).

### Hosted vs.Hybrid environments

The table below highlights the main differences between hosted and hybrid environments.

{: .table .table-bordered .table-hover}
| Functionality           |Feature             |  Hosted                    | Hybrid |
| --------------          | --------------     |---------------             | --------------- |
| Runtime                 | Installation       | Provisioned by Codefresh   | Provisioned by customer       |
|                         | Runtime cluster    | Managed by Codefresh       | Managed by customer       |
|                         | Number per account | One runtime                | Multiple runtimes            |
|                         | External cluster   | Managed by customer        | Managed by customer         |
|                         | Upgrade            | Managed by Codefresh       | Managed by customer |
|                         | Uninstall          | Managed by customer        | Managed by customer |
| Argo CD                 |                    | Codefresh cluster          | Customer cluster  |
| CI Ops                  | Delivery Pipelines |Not supported               | Supported  |
|                         |Workflows           | Not supported              | Supported  |
|                         |Workflow Templates  | Not supported              | Supported  |
| CD  Ops                 |Applications        | Supported                  | Supported |
|                         |Image enrichment    | Supported                  | Supported  |
|                         | Rollouts           | Supported                  |  Supported  |
|Integrations             |                    | Supported                  | Supported  |
|Dashboards               |Home Analytics      | Hosted runtime and deployments|Runtimes, deployments, Delivery Pipelines |
|                         |DORA metrics        | Supported                 |Supported        |
|                         |Applications        | Supported                 |Supported        |