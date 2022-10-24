---
title: "Architecture"
description: ""
group: getting-started
toc: true
---

Codefresh GitOps is built around an enterprise version of the Argo Ecosystem, fully GitOps-compliant with industry-standard security.
To cater to differing requirements and degrees of enterprise security, Codefresh offers hosted and hybrid installation environments.    
Both hosted and hybrid environments share a similar architecture, with the key difference being the location of the Codefresh Runtime.

The sections that follow illustrate hosted and hybrid architecture, and describe the main components in the Codefresh Platform.

### Codefresh architecture

The diagram shows a high-level view of the Codefresh architecture. 
As you can see, the Codefresh Control Plane, the Codefresh Runtime, and the Codefresh Clients are core components in the Codefresh solution. 
Additional components are installed depending on the type of runtime selected, and are indicated as such.

{% include
   image.html
   lightbox="true"
   file="/images/getting-started/architecture/arch-codefresh-simple.png"
 url="/images/getting-started/architecture/arch-codefresh-simple.png"
  alt="Codefresh architecture"
  caption="Codefresh architecture"
  max-width="100%"
%}

### Codefresh Control Plane
The Codefresh Control Plane is the SaaS component in the platform. External to the enterprise firewall, it does not have direct communication with the Codefresh Runtime, Codefresh Clients, or the customer's organizational systems. The Codefresh Application Proxy and the Codefresh Clients communicate with the Codefresh Control Plane to retrieve the required information.  

The Codefresh Control Plane:  

* Securely stores user accounts   
* Enforces the permissions model 
* Controls authentication, user management, and billing

#### Codefresh Tunnel Server
The Codefresh Tunnel Server is installed in the Codefresh platform for _ingress-less runtime environments_, together with the Codefresh Tunnel Client and the Codefresh Tunnel Router.  
A fast reverse proxy (FRP), the Codefresh Tunnel Server communicates with the enterprise cluster located behind a NAT or firewall.  
It comprises the following containers:  
* Codefresh FRP Server: Forwards the traffic from Codefresh clients to the customer cluster, and manages the lifecycle with the Codefresh Tunnel Client.
* Codefresh FRP Plugin: Implements Codefresh logic to authenticate requests from the Codefresh Tunnel Client to open tunneling connections with the Tunnel Server. It runs as a sidecar container.

#### Codefresh Tunnel Router
The Codefresh Tunnel Router is installed in the Codefresh platform for _ingress-less runtime environments_, together with the Codefresh Tunnel Server and the Codefresh Tunnel Router. Installed as a service, the Codefresh Tunnel Router maps the user traffic from Codefresh clients and routes it to the pod that the required tunnel is connected to. 




### Codefresh Runtime
The Codefresh Runtime is installed on a Kubernetes cluster, and houses the enterprise distribution of the Codefresh Application Proxy and the Argo Project.  
It is installed on a cluster either in the Codefresh platform or in the customer environment:
* Hosted runtimes: Installed on a _Codefresh-managed cluster_ in the Codefresh platform.
* Hybrid runtimes: Installed on a _customer-managed cluster_ in the customer environment.
  Hybrid runtimes can be installed with or without an ingress controller.  
  * Ingress-less runtimes: Optimal when the target cluster is not exposed to the internet. Uses FRP tunneling for Codefresh Runtime-customer cluster communication.
  * Ingress runtimes: Optimal when the the target cluster is exposed to the internet. Uses an ingress controller for Codefresh Runtime-customer cluster communication.
 

The Codefresh Runtime includes:  
* Codefresh Application Proxy
* Argo Project
* Codefresh Tunneling Client


The Codefresh Runtime: 
* Integrates with Argo Workflows and Argo Events to run Delivery Pipelines (hybrid environment), and with Argo CD and Argo Rollouts (both hosted and hybrid environments) to implement GitOps deployments for continuous deployments and progressive delivery.
* Ensures that the installation repository and the Git Sources are always in sync, and applies Git changes back to the cluster.
* Receives events and information from the customer's organizational systems to execute workflows.

#### Codefresh Application Proxy
The Codefresh Application Proxy (App-Proxy) functions as the Codefresh agent, and is deployed as a service in the Codefresh Runtime. 
For runtimes with ingress controllers, the App-Proxy is the single point-of-contact between the Codefresh Runtime, and the Codefresh Clients, the Codefresh Platform, and any organizational systems in the customer environment.  
The App-Proxy is exposed externally through ingress controllers/load-balancers. 

  
The App-Proxy:  
* Accepts and serves requests from Codefresh Clients either via the Codefresh UI or CLI 
* Retrieves a list of Git repositories for visualization in Codefresh Clients  
* Retrieves permissions from the Codefresh Control Plane to authenticate and authorize users for the required operations.    
* Implements commits for GitOps-controlled entities, such as Delivery Pipelines and other CI resources
* Implements state-change operations for non-GitOps controlled entities, such as terminating Argo Workflows

{::nomarkdown}
<br>
{:/}


#### Codefresh Tunnel Client
The Codefresh Tunnel Client is required for ingress-less runtime environments, similar to the Codefresh Tunnel Server and the Tunnel Routing Service. It functions as the Codefresh agent for the ingress-less hybrid runtime. Installed in the Codefresh Runtime, it establishes the tunneling connection to the Tunnel Server through the TCP binding port exposed by the Tunnel Server. 

A single Codefresh Runtime can have more than one Tunnel Client, with all or some of these connecting to the same Tunnel Server. 


The Tunnel Client: 
* Initiates the connection with the Tunnel Server.
* Forwards the incoming traffic from the Tunnel Server using internal reverse proxy to App-Proxy, Argo Project services, and other services located internally.  

{::nomarkdown}
<br>
{:/}

#### Argo Project 

The Argo Project includes:
* Argo CD for declarative continuous deployment 
* Argo Rollouts for progressive delivery 
* Argo Workflows as the workflow engine 
* Argo Events for event-driven workflow automation framework


### Codefresh Clients

Codefresh Clients include the Codefresh UI and the Codefresh CLI.  

#### Codefresh UI

The Codefresh UI provides a unified, enterprise-wide view of your deployment (runtimes and clusters), and CI/CD operations (Delivery Pipelines, workflows, and deployments) in the same location.  

* Multi-runtime and multi-cluster management: View all provisioned runtimes, and the clusters they manage in the Runtimes page.   
* Dashboards for CI and CD visualizations: The Home dashboard for critical insights into CI and CD lifecycles, the DORA metrics dashboard for DevOps metrics, the Applications dashboard for GitOps details, and the Delivery Pipelines dashboard for workflow details.
* Wizards to simplify installation, Delivery Pipeline and application creation and management.
* Integrations for software delivery workflows  

{::nomarkdown}
<br>
{:/}

#### Codefresh CLI 

Perform hybrid runtime installation, and runtime and cluster management operations.

{::nomarkdown}
<br><br>
{:/}

### Customer environment
The customer environment that communicates with the Codefresh platform, generally includes:
* Ingress controller for hybrid runtimes
* Managed clusters
* Organizational systems

#### Ingress Controller
The ingress controller is is configured on the same Kubernetes cluster as the Codefresh Runtime in hybrid runtime environments. It implements the ingress traffic rules for the Codefresh Runtime. 
See [Ingress controller requirements]({{site.baseurl}}/docs/runtime/requirements/#ingress-controller).

{::nomarkdown}
<br>
{:/}

#### Managed clusters
Managed clusters are external clusters registered to provisioned hosted or hybrid runtimes.  

* Hosted runtime: Requires you to connect to an external K8s cluster as part of setting up the Hosted GitOps environment. You can add more managed clusters after completing the setup.
* Hybrid runtimes: You can add external clusters after provisioning hybrid runtimes.  

See [Add external clusters to runtimes]({{site.baseurl}}/docs/runtime/managed-cluster/).

{::nomarkdown}
<br>
{:/}
                
#### Organizational Systems
Organizational Systems include the tracking, monitoring, notification, container <!registries, Git providers, and other tools incorporated into the continuous integration and continuous deployment processes. They can be entirely on-premises or in the public cloud.   
<!---need to update this-->For hybrid runtime environments with ingress controllers, these tools send events to the Codefresh Application Proxy via the ingress controller to trigger and manage CI/CD flows. For ingress-less hybrid runtimes, all events are routed by the Codefresh Tunnel Server to the Codefresh Tunnel Client which in turn forwards the events to the relevant service.


### Hosted GitOps runtime architecture
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

### Hybrid runtime architecture _with ingress_
In the hybrid environment, the Codefresh Runtime is located on the customer's K8s cluster, and managed by the customer. 

> Hybrid runtimes can be installed without an ingress controller. See [Hybrid runtime architecture _without ingress_](#hybrid-runtime-architecture-without-ingress)


{% include
   image.html
   lightbox="true"
   file="/images/getting-started/architecture/arch-hybrid-ingress.png"
 url="/images/getting-started/architecture/arch-hybrid-ingress.png"
  alt="Hybrid runtime architecture _with_ ingress controller"
  caption="Hybrid runtime architecture _with_ ingress controller"
  max-width="100%"
%}

### Hybrid runtime architecture _without ingress_

> Hybrid runtimes can be installed with an ingress controller. See [Hybrid runtime architecture _with ingress_](#hybrid-runtime-architecture-without-ingress_)

{% include
   image.html
   lightbox="true"
   file="/images/getting-started/architecture/arch-hybrid-ingressless.png"
 url="/images/getting-started/architecture/arch-hybrid-ingressless.png"
  alt="Hybrid runtime architecture _without_ ingress controller"
  caption="Hybrid runtime architecture _without_ ingress controller"
  max-width="100%"
%}

### Codefresh Platform
The Codefresh Platform comprises:  

* Codefresh Control Plane 
* Codefresh Runtime with the Codefresh Application Proxy and Argo Project
* Codefresh Clients, the Codefresh UI and the Codefresh CLI

{::nomarkdown}
<br><br>
{:/}




{::nomarkdown}
<br>
{:/}

#### Codefresh Application Proxy




#### Codefresh Clients




### Related articles
[Set up a hosted runtime environment]({{site.baseurl}}/docs/runtime/hosted-runtime/)  
[Install a hybrid runtime]({{site.baseurl}}/docs/runtime/installation/)



