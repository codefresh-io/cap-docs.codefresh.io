---
title: "Installation environments"
description: ""
group: runtime
toc: true
---

Codefresh supports two installation environments:

* **Hosted** installation, where the runtime is hosted in Codefresh and managed by Codefresh.  
  Hosted enviroments are full-cloud environments, where all updates and improvements are managed by Codefresh, with zero-maintenance overhead for you as the customer. 
  For more information on how to set up the hosted installation, including provisioning hosted runtimes, see [Set up Hosted GitOps]({{site.baseurl}}/docs/incubation/hosted-runtime/).  

* **Hybrid** installation, where the runtime is installed in the customer premises, and managed by the customer.  
  Hybrid environments are ideal for organizations that want their source code within their premises, or have other security constraints. The hybrid installation strikes the perfect balance between security, flexibility, and ease of use. As the custome, you are responsible for runtime installation and upgrade, whike Codefresh continues to maintain most aspects of the platform.  
  For more information on hybrid runtimes, start with [Hybrid runtime requirements]({{site.baseurl}}/docs/runtime/requirements/), and continue with [Installling hybrid runtimes]({{site.baseurl}}/docs/runtime/installation/).  
  
### Hosted vs.hybrid environments

The table below highlights the main differences between hosted and hybrid environments.

{: .table .table-bordered .table-hover}
| Functionality           |Feature             |  Hosted                    | Hybrid |
| --------------          | --------------     |---------------             | --------------- |
| Runtime                 | Installation       | Provisioned by Codefresh   | Provisioned by customer       |
|                         | Runtime cluster    | Managed by Codefresh       | Managed by customer       |
|                         | Number per account | Only one runtime           | Multiple runtimes            |
|                         | External cluster   | Managed by customer        | Managed by customer         |
|                         | Upgrade            | Managed by Codefresh       | Managed by customer |
|                         | Uninstall          | Managed by customer        | Managed by customer |
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