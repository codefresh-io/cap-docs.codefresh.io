---
title: "Architecture"
description: ""
group: getting-started
toc: true
---

Codefresh GitOps is built around an enterprise version of the Argo Ecosystem,  that is fully GitOps-compliant, with industry-standard security.
The architecture diagram below illustrates the generic architecture of the Codefresh platform components. 


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
The Codefresh Platform is the SaaS component in the solution. Located outside the entperise firewall, it does not have direct communication with the Codefresh Runtime, Codefresh Clients, or the organization systems. The Codefresh Application Proxy and the Codefresf Clients retrieve  information from the Codefresh Platform as required.

The Codefresh platform:
* Securely stores user accounts, and retrieves   
* Enforces the permissions model 
* Controls authentication, user management, and billing

### Codefresh Runtime
The Codefresh Runtime is installed on a Kubernetes cluster, and houses the enterprise distribution of the Argo Project and the Codefresh Application Proxy.  
To cater to differing requirements and degrees of enterprise security, Codefresh offers hosted and hybrid runtime installations:  

* Hosted runtimes are installed on a _Codefresh-managed cluster_ in the Codefresh platform
* Hybrid runtimes are installed on a _customer-managed cluster_. 

The Codefresh Runtime:
* Integrates with Argo Workflows and Argo Events to run Delivery Pipelines, and with Argo CD and Argo Rollouts to implement GitOps deployments and progressive delivery
* Ensures that the installation repository and the Git Sources are always in sync, and applies Git changes back to the cluster
* Receives events and information from the user's organization systems to execute workflows

#### Codefresh Application Proxy
The Codefresh Application Proxy (App-Proxy) functions as the Codefresh agent. Deployed as a service in the Codefresh Runtime, the App-proxy is exposed externally through ingress controllers/load-balancers. It is the single point-of-contact from the Codefresh Clients, the Codefresh Platform, and any organizational system to the Codefresh Runtime. 

**Routing**  

The App-Proxy:
* Accepts and serves requests from Codefresh Clients either via the Codefresh UI or CLI. 
* Retrieves a list of Git repositories for visualization in Codefresh Clients

**Authentication and authorization**  
The App-Proxy retreives permissions from the Codefresh platform to authenticate and authorize users for the required operations.  


**Write operations**
The App-Proxy performs write and state-change operations:
* Commits for GitOps-controlled entities, such as Delivery Pipelines and other CI resources
* State-change operations for non-GitOps controlled entities, such as terminating Argo Workflows


#### Argo Project 

The Argo Project includes:
* Argo CD for declarative continuous deployment 
* Argo Rollouts for progressive delivery 
* Argo Workflows as the workflow engine 
* Argo Events for event-driven workflow automation framework

### Codefresh Clients
Codefresh Clients include the Codefresh UI and the Codefresh CLI.

**Codefresh UI**
The Codefresh UI provides a unified, enterprise-wide view of your deployment (runtimes and clusters), and CI/CD operations (Delivery Pipelines, workflows, and deployments) in the same location.
* Runtime and managed clusters: View all provisioned runtimes, and the clusters they manage in a tabbed view in the Runtimes page.   
* Dashboards for CI and CD visualizations: Starting with the Home dashboard for critical insights into CI and CD lifecycles, the DORA metrics dashboard for DevOps metrics, the Applications dashboard for GitOps details, and the Delivery Pipelines dashboard for workflow details.
* Wizards to simplify installation, Delivery Pipeline and application creation and mamagment.
* Integrations for software delivery workflows

**Codefresh CLI**
Perform runtime and cluster management operations.


### Ingress Controller
For hybrid runtime installations, the ingress controller implements the ingress traffic rules for the Codefresh Runtime. It is configured on the same Kubernetes cluster as the Codefresh Runtime. Codefresh supports popular ingress controllers, such as Amabassador, NGINX Enterprise, Istio and Trafix. See [Ingress controller]({{site.baseurl}}/docs/runtime/requirements/#ingress-controller).
                
### Organizational Systems
Organizational systems include the tracking, monitoring, notification, container registeries, Git providers, and other tools incorportated into the continuous integration and continuous deployment processes. They can be entirely on-premises or in the public cloud.   
The tools send events to the Codefresh Application Proxy (via the ingress controller) to trigger and manage CI/CD flows. 

### Related articles
[Set up a hosted runtime environment]({{site.baseurl}}/docs/runtime/hosted-runtime/)  
[Install hybrid runtimes]({{site.baseurl}}/docs/runtime/installation/)



