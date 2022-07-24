---
title: "Installation environments"
description: ""
group: runtime
toc: true
---

Codefresh supports two installation environments:


* **Hosted** environments, with Argo CD installed in the Codefresh cluster (Beta) 
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

* **Hybrid** environments, with Argo CD installed in the customer's cluster  
  The runtime is installed in the customer's cluster, and managed by the customer.  
  Hybrid environments are ideal for organizations that want their source code within their premises, or have other security constraints. Hybrid installations strike the perfect balance between security, flexibility, and ease of use. As the customer, you are responsible for installing and upgrading runtimes, while Codefresh continues to maintain most aspects of the platform.  
 
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

  **Codefresh Runtime installations**

See xref to runtime installation options
 to Git repositories, the Codefresh runtime installation repository, Codefresh Git Sources, and the Codefresh shared configuration repository. It does so for two types of entities:
**Codefresh Runtime finctionality**
The runtime:
* Ensures that the installation repository and the Git Sources are always in sync, and applies Git changes back to the cluster
* Receives events and information from the user's organization systems to execute workflows
  
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