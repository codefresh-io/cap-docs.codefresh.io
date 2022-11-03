---
title: "Architecture"
description: ""
group: getting-started
toc: true
---

Codefresh GitOps is built around an enterprise version of the Argo Ecosystem, fully compliant with the GitOps paradigm, with industry-standard security.
To cater to differing requirements and degrees of enterprise security, Codefresh supports hosted and hybrid installation environments. 

Hosted environments: SaaS 

Hybrid environments:



The sections that follow illustrate the architectures of the different installation environemtns, starting with the high-level overview of the Codefresh Platform,  amd  describe the main components in the Codefresh Platform.

### Codefresh architecture

The diagram shows a high-level overview of the Codefresh Platform. 
As you can see, the Codefresh Control Plane, the Codefresh Runtime, and the Codefresh Clients are core components in the Codefresh solution. 
Additional components are installed depending on the type of runtime selected.

{% include
   image.html
   lightbox="true"
   file="/images/getting-started/architecture/arch-codefresh-simple.png"
 url="/images/getting-started/architecture/arch-codefresh-simple.png"
  alt="Codefresh architecture"
  caption="Codefresh architecture"
  max-width="100%"
%}

{::nomarkdown}
<br>
{:/}

#### Codefresh Control Plane
The Codefresh Control Plane is the SaaS component in the platform. External to the enterprise firewall, it does not have direct communication with the Codefresh Runtime, Codefresh Clients, or the customer's organizational systems. The Codefresh Application Proxy and the Codefresh Clients communicate with the Codefresh Control Plane to retrieve the required information.  

The Codefresh Control Plane:  

* Securely stores user accounts   
* Enforces the permissions model 
* Controls authentication, user management, and billing

{::nomarkdown}
<br>
{:/}

#### Codefresh Runtime
The Codefresh Runtime is installed on a Kubernetes cluster, and houses the enterprise distribution of the Codefresh Application Proxy and the Argo Project.  
Depending on the type of installation environment, the Codefresh Runtime is installed either in the Codefresh platform or in the customer environment:
* Hosted environments: Installed on a _Codefresh-managed cluster_ in the Codefresh platform.
* Hybrid environments: Installed on a _customer-managed cluster_ in the customer environment.
  In hybrid environments, you can install the Codefresh runtimes with or without an ingress controller.  
  * Ingress-less runtimes: Optimal when the target cluster is not exposed to the internet. Uses FRP (Fast Reverse Proxy) tunneling for communication between the Codefresh Runtime in the customer cluster and the Codefresh Platform.
  * Ingress-based runtimes: Optimal when the target cluster is exposed to the internet. Uses an ingress controller for communication between the Codefresh Runtime in the customer cluster and the Codefresh Platform.
 
The Codefresh Runtime: 
* Integrates with Argo Workflows and Argo Events to run Delivery Pipelines (hybrid environments), and with Argo CD and Argo Rollouts (both hosted and hybrid environments) to implement GitOps-compatible continuous deployment and progressive delivery.
* Ensures that the installation repository and the Git Sources are always in sync, and applies Git changes back to the target cluster.
* Receives events and information from the customer's organizational systems to execute workflows.

{::nomarkdown}
<br>
{:/}
#### Codefresh Clients

Codefresh Clients include the Codefresh UI and the Codefresh CLI.  


* Codefresh UI  
  The Codefresh UI provides a unified, enterprise-wide view of deployments (runtimes and clusters), and CI/CD operations (Delivery Pipelines, workflows, and deployments) in the same location.  

  * Multi-runtime and multi-cluster management: View all provisioned runtimes, and the clusters they manage in the Runtimes page.   
  * Dashboards for CI and CD visualizations: The Home dashboard for critical insights into CI and CD lifecycles, the DORA metrics dashboard for DevOps metrics, the Applications dashboard for GitOps details, and the Delivery Pipelines dashboard for workflow details.
  * Wizards to simplify installation, Delivery Pipeline and application creation and management.
  * Integrations for software delivery workflows  


* Codefresh CLI  
  The Codefresh CLI includes commands to install hybrid runtimes, add external clusters, and manage runtimes and clusters.

### Codefresh runtime architecture
Zoom in  we can zoom in on the different archiectures and components in Codefresh Runtimes.

* Hosted GitOps runtime architecture
* Hybrid runtime architecure:
  * [Ingress-based](#ingress-based-hybrid-runtime-architecture)
  * [Ingress-less](#ingress-less-hybrid-runtime-architecture)
* Runtime components
  * 

#### Hosted GitOps runtime architecture
In the hosted environment, the Codefresh Runtime is located on a K8s cluster managed by Codefresh. 

{% include
   image.html
   lightbox="true"
   file="/images/getting-started/architecture/arch-hosted.png"
 url="/images/getting-started/architecture/arch-hosted.png"
  alt="Hosted runtime architecture"
  caption="Hosted runtime architecture"
  max-width="100%"
%}

#### Ingress-based hybrid runtime architecture
For ingress-based runtimes in hybrid installation environments, the Codefresh Runtime is located on the customer's K8s cluster, and managed by the customer. An Ingress Controller controls communication between ethe Code from the customer's organizational systems, Codefresh and Git clients, and fro



{% include
   image.html
   lightbox="true"
   file="/images/getting-started/architecture/arch-hybrid-ingress.png"
 url="/images/getting-started/architecture/arch-hybrid-ingress.png"
  alt="Ingress-based hybrid runtime architecture"
  caption="Ingress-based hybrid runtime architecture"
  max-width="100%"
%}

### Ingress-less hybrid runtime architecture

> Hybrid runtimes can be installed with an ingress controller. See 

{% include
   image.html
   lightbox="true"
   file="/images/getting-started/architecture/arch-hybrid-ingressless.png"
 url="/images/getting-started/architecture/arch-hybrid-ingressless.png"
  alt="Ingress-less hybrid runtime architecture"
  caption="Ingress-less hybrid runtime architecture"
  max-width="100%"
%}



#### Codefresh Application Proxy
The Codefresh Application Proxy (App-Proxy) functions as the Codefresh agent, and is deployed as a service in the Codefresh Runtime. It App-Proxy is exposed externally through ingress controllers/load-balancers.  
For ingress-based hybrid runtimes, the App-Proxy is the single point-of-contact between the Codefresh Runtime, and the Codefresh Clients, the Codefresh Platform, and any organizational systems in the customer environment.    
 
The App-Proxy:  
* Accepts and serves requests from Codefresh Clients either via the Codefresh UI or CLI 
* Retrieves a list of Git repositories for visualization in Codefresh Clients  
* Retrieves permissions from the Codefresh Control Plane to authenticate and authorize users for the required operations.    
* Implements commits for GitOps-controlled entities, such as Delivery Pipelines and other CI resources
* Implements state-change operations for non-GitOps controlled entities, such as terminating Argo Workflows

{::nomarkdown}
<br>
{:/}

#### Argo Project 

The Argo Project includes:
* Argo CD for declarative continuous deployment 
* Argo Rollouts for progressive delivery 
* Argo Workflows as the workflow engine 
* Argo Events for event-driven workflow automation framework


{::nomarkdown}
<br><br>
{:/}


##### Codefresh Tunnel Server
Applies only to _ingress-less_ runtimes in hybrid installation environments.  
The Codefresh Tunnel Server is installed in the Codefresh platform. It communicates with the enterprise cluster located behind a NAT or firewall.  

The Tunnel Server:  
* Forwards traffic from Codefresh Clients to the client (customer) cluster.
* Manages the lifecycle of the Codefresh Tunnel Client.
* Authenticates requests from the Codefresh Tunnel Client to open tunneling connections.

{::nomarkdown}
<br>
{:/}

#### Codefresh Tunnel Client
Applies only to _ingress-less_ runtimes in hybrid installation environments.  

The Codefresh Tunnel Client functions as the Codefresh agent. Installed in the Codefresh Runtime, it establishes the tunneling connection to the Codefresh Tunnel Server via the WebSocket Secure (WSS) protocol.   
A single Codefresh Runtime can have a single Tunnel Client.  

The Codefresh Tunnel Client: 
* Initiates the connection with the Codefresh Tunnel Server.
* Forwards the incoming traffic from the Tunnel Server using internal reverse proxy to App-Proxy, and other services.  

{::nomarkdown}
<br>
{:/}



### Customer environment
The customer environment that communicates with the Codefresh platform, generally includes:
* Ingress controller for hybrid runtimes  
  The ingress controller is configured on the same Kubernetes cluster as the Codefresh Runtime in hybrid runtime environments. It implements the ingress traffic rules for the Codefresh Runtime. 
  See [Ingress controller requirements]({{site.baseurl}}/docs/runtime/requirements/#ingress-controller).
* Managed clusters  
  Managed clusters are external clusters registered to provisioned hosted or hybrid runtimes to which to deploy applications.  
  * Hosted runtime: Requires you to connect at least one external K8s cluster as part of setting up the Hosted GitOps environment. 
  * Hybrid runtimes: You can add external clusters after provisioning hybrid runtimes. 
  See [Add external clusters to runtimes]({{site.baseurl}}/docs/runtime/managed-cluster/).
* Organizational systems  
  Organizational Systems include the customer's tracking, monitoring, notification, container registries, Git providers, and other systems. They can be entirely on-premises or in the public cloud.   
  Either the Ingress Controller (ingress-based hybrid environments), or the Codefresh Tunnel Client (ingress-less hybrid environments), forwards incoming events to the Codefresh Application Proxy. 

### Related articles
[Set up a hosted runtime environment]({{site.baseurl}}/docs/runtime/hosted-runtime/)  
[Install a hybrid runtime]({{site.baseurl}}/docs/runtime/installation/)



