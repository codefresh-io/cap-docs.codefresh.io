---
title: "Architecture"
description: ""
group: getting-started
toc: true
---

The Codefresh GitOps platform is built around an enterprise version of the Argo Ecosystem, that is fully GitOps-compliant, with industry-standard security.


{% include
image.html
lightbox="true"
file="/images/getting-started/architecture/simple-architecture.png"
url="/images/getting-started/architecture/simple-architecture.png"
alt="Codefresh architecture"
caption="Codefresh architecture"
max-width="100%"
%}

### Codefresh Platform 
The Codefresh Platform is the SaaS component in the solution. Located outside the entperise firewall, it does not have direct communication with the Codefresh Runtime, Codefresh Clients, or the organization systems. 

The Codefresh platform:
* Securely stores user accounts, and retrieves   
* Enforces the permissions model 
* Controls authentication, user management, and billing

### Codefresh Runtime
The Codefresh Runtime is installed on a Kubernetes cluster, and houses the enterprise distribution of the Argo Project and the Codefresh Application Proxy.  
Codefresh offers hosted and hybrid runtime installations to cater to differing requirements and degrees of enterprise security:
* Hosted runtimes are installed on a _Codefresh-managed cluster_ in the Codefresh platform
* Hybrid runtimes are installed on a _customer-managed cluster_

Codefresh Runtime tools are completely integrated with all Argo components and  simplify and enhance their operations. By building on and integrating Argo Workflows and Events for running delivery pipelines, and Argo CD and Rollouts for GitOps deployments and progressive delivery we’re able to provide a stronger security model with a greatly simplified management that works at scale.

#### Codefresh Application Proxy
The Codefresh Application Proxy (App-proxy) functions as the Codefresh agent within the Codefresh Runtime. Deployed as a service, the App-proxy is exposed externally through ingress controllers and load-balancers, and is the point-of-contact for the Codefresh Clients, the Codefresh Platform, and any organizational system that customers use for collaboration, issue tracking and more. 

**Routing**  

The app-proxy:

* Accepts and serves requests from Codefresh Clients either via the Codefresh UI or CLI. 
* Retrieves a list of Git repositories for visualization in Codefresh Clients


**Authentication and authorization**  

The App-proxy gets permissions from the Codefresh platform to authenticate and authorize users for the required operations.  


**Write operations**
The App-proxy performs write operations state-change operations:
* Commits for GitOps-controlled entities, such as Delivery Pipelines and other CI resources
* State-change operations for non-GitOps controlled entities, such as terminating Argo Workflows


#### Argo Project 

Argo CD, a declarative GitOps application deployments in the appplication dashabord
connectts to Git repositorues and correlate deployment of resources in GitOps

Argo Rollouts

Argo Workflows
basically provides workflow-related execustions capabilities in Codefrsh for Delivery Pipelines 
CI/CD flows


Argo Events

Trigger events incuding  argo workflow Git
### Codefresh Clients
Codefresh Clients include the Codefresh UI and the Codefresh CLI.

**Codefresh UI**
The Codefresh UI provides a unified view of your enterprise deployment, and CI/CD operations in the same, centralized location.
* Runtime and managed clusters: View all provisioned runtimes, and the clusters they manage.  across clusters, and  informatSingle UI for all clusters  
* Dashboards: 
* Wizards simplify installation, Delivery pipeline and application creation. In contrast to Argo CD which has a UI per cluster, the Codefresh UI 
Key UI 
The Home dashboard is your entry to the Giving you a complete picture of your runtimes, managed clusters, builds, and deployments with critical insights. You can easily filter these views and drill down for more details into any area of your software delivery process. Truly an enterprise-wide real-time display of your DevOps process.

Applications dashbaord is the GitOps deployment dashbaord 

Centralized management of Argo, Git, logins, and secrets
Unified interface for code-to-cloud visibility
Argo component intercompatibility testing
Rigorous security validations
Extended functionality to manage application lifecycles
Key native integrations for software delivery workflows
Traceability from source code to artifact to endpoint
Integrate project management into the release life



### Ingress Controller
For hybrid runtime installations, the Ingress controller is configured on the same Kubernetes cluster as the Codefresh Runtime. Codefresh supports popular ingress controllers, such as Amabassador, NGINX Enterprise, Istio and Trafix.  compaitble with Kubernetes. See X_REF

The ingress controller implements the ingress traffic rules for the Codefresh Runtime.
See X-REF


### Organizational Systems
Organizational systems incude the tracking, monitoring, notification, container registeries, Git providers, and other tools sends events and information to the Codefresh Application Proxy to trigger and manage CI/CD flows. 


They can be entirely on-premises or in the public cloud. 


