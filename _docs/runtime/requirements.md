---
title: "Requirements"
description: ""
group: runtime
toc: true
---


The requirements listed are the **_minimum_** requirements for CSDP (Codefresh Software Delivery Platform) runtimes.  

> In the documentation, Kubernetes and K8s are used interchangeably. 


### Kubernetes cluster requirements
This section lists cluster requirements.

#### Cluster version
Kubernetes cluster, server version 1.20 or higher, without Argo Project components.
> Tip:  
>  Run `kubectl version --short`, and check the server version.


#### Ingress controller
* Ingress controller in cluster  
  Configure your Kubernetes cluster with an Ingress controller component that is exposed from the cluster. Currently, we support the `NGINX` ingress controller.  
  > Tip:   
  >  Verify that the ingress controller has a valid external IP address.  
  >  Run `kubectl get svc ingress-nginx-controller -n ingress-nginx`, and verify that the EXTERNAL-IP column shows a valid hostname. 

* Valid SSL certificate  
  The ingress controller must have a valid SSL certificate from an authorized CA (Certificate Authority) for secure runtime installation.  


#### Provider Specific Instrucitons

Codefresh Software Delivery Platform is tested and supported in major providers. For convenience, provider-specific instructions for providers both supported and untested are included below.

##### AWS

**Using Ingress-Nginx**
1. Apply `kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/aws/deploy.yaml`
1. Verify a valid external address exists `kubectl get svc ingress-nginx-controller -n ingress-nginx`

See additional configuration options in the [ingress-nginx documentation for AWS](https://kubernetes.github.io/ingress-nginx/deploy/#aws).

##### Azure (AKS)
**Using Ingress-Nginx**
1. Apply `kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/cloud/deploy.yaml`
1. Verify a valid external address exists `kubectl get svc ingress-nginx-controller -n ingress-nginx`

See additional configuration options in the [ingress-nginx documentation for AKS](https://docs.microsoft.com/en-us/azure/aks/ingress-internal-ip?tabs=azure-cli#create-an-ingress-controller).

##### Bare Metal Clusters

##### Digital Ocean
**Using Ingress-Nginx**
1. Apply `kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/do/deploy.yaml`
1. Verify a valid external address exists `kubectl get svc ingress-nginx-controller -n ingress-nginx`

See additional configuration options in the [ingress-nginx documentation for Digital Ocean](https://kubernetes.github.io/ingress-nginx/deploy/#digital-ocean).

##### Docker Desktop
**Using Ingress-Nginx**
1. Apply `kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/cloud/deploy.yaml`
1. Verify a valid external address exists `kubectl get svc ingress-nginx-controller -n ingress-nginx`

See additional configuration options in the [ingress-nginx documentation for Docker Desktop](https://kubernetes.github.io/ingress-nginx/deploy/#docker-desktop). **Note** By default, Docker Desktop services will provision with localhost as their external address. Triggers in delivery pipelines will not be able to reach this instance unless they originate from the same machine where Docker Desktop is being used.

##### Exoscale
**Using Ingress-Nginx**
1. Apply `kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/exoscale/deploy.yaml`
1. Verify a valid external address exists `kubectl get svc ingress-nginx-controller -n ingress-nginx`

See additional configuration options in the [ingress-nginx documentation for Exoscale](https://github.com/exoscale/exoscale-cloud-controller-manager/blob/master/docs/service-loadbalancer.md).

##### Google (GKE)
**Adding Firewall Rules**
GKE by default limits outbound requests from nodes. For the runtime to communicate with the control-plane in CSDP, add a firewall-specific rule.

1. Find your cluster's network:   
  `gcloud container clusters describe [CLUSTER_NAME] --format=get"(network)"`
1. Get the Cluster IPV4 CIDR:  
  `gcloud container clusters describe [CLUSTER_NAME] --format=get"(clusterIpv4Cidr)"`  
1. Replace the `[CLUSTER_NAME]`, `[NETWORK]`, and `[CLUSTER_IPV4_CIDR]`, with the relevant values:  
   ```
    gcloud compute firewall-rules create "[CLUSTER_NAME]-to-all-vms-on-network" \
    --network="[NETWORK]" \
    --source-ranges="[CLUSTER_IPV4_CIDR]" \
    --allow=tcp,udp,icmp,esp,ah,sctp
    ```

**Using Ingress-Nginx**
1. Create a `cluster-admin` role binding ```kubectl create clusterrolebinding cluster-admin-binding \
  --clusterrole cluster-admin \
  --user $(gcloud config get-value account)```
1. Apply `kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/cloud/deploy.yaml`
1. Verify a valid external address exists `kubectl get svc ingress-nginx-controller -n ingress-nginx`

We recommend reviewing the provider [specific documentation for GKE](https://kubernetes.github.io/ingress-nginx/deploy/#gce-gke).

##### MicroK8s
1. Install using Microk8s addon system `microk8s enable ingress`
1. Verify a valid external address exists `kubectl get svc ingress-nginx-controller -n ingress-nginx`

MicroK8s has not been tested with CSDP and may require additional configuration. Ingress addon [documentation can be found here](https://microk8s.io/docs/addon-ingress).

##### MiniKube
1. Install using MiniKube addon system `minikube addons enable ingress`
1. Verify a valid external address exists `kubectl get svc ingress-nginx-controller -n ingress-nginx`

MiniKube has not been tested with CSDP and may require additional configuration. Ingress addon [documentation can be found here](https://kubernetes.github.io/ingress-nginx/deploy/#minikube).

##### Oracle Cloud Infrastructure
**Using Ingress-Nginx**
1. Apply `kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/cloud/deploy.yaml`
1. Verify a valid external address exists `kubectl get svc ingress-nginx-controller -n ingress-nginx`

See additional configuration options in the [ingress-nginx documentation for Oracle Cloud](https://kubernetes.github.io/ingress-nginx/deploy/#oracle-cloud-infrastructure).


##### Scale Away
**Using Ingress-Nginx**
1. Apply `kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/scw/deploy.yaml`
1. Verify a valid external address exists `kubectl get svc ingress-nginx-controller -n ingress-nginx`

See additional configuration options in the [ingress-nginx documentation for Scaleway](https://kubernetes.github.io/ingress-nginx/deploy/#scaleway).


#### Node requirements
* Memory: 5000 MB
* CPU: 2

#### Runtime namespace permissions for resources

{: .table .table-bordered .table-hover}
|  Resource                   |  Permissions Required|  
| --------------            | --------------           |  
| `ServiceAccount`            | Create, Delete         |                             
| `ConfigMap`                 | Create, Update, Delete |          
| `Service`                   | Create, Update, Delete |       
| `Role`                       | In group `rbac.authorization.k8s.io`: Create, Update, Delete |       
| `RoleBinding`               | In group `rbac.authorization.k8s.io`: Create, Update, Delete  | 
| `persistentvolumeclaims`    | Create, Update, Delete               |   
| `pods`                       | Creat, Update, Delete               | 

### Git repository requirements
This section lists the requirements for Git installation repositories.

#### Git installation repo
If you are using an existing repo, make sure it is empty.

#### Git access tokens
CSDP requires personal two access tokens, one for runtime installation, and the other for your Git actions in CSDP. 

##### Git runtime token
The Git runtime token is mandatory for runtime installation.

The token must have valid:
  * Expiration date: Default is `30 days`  
  * Scopes: `repo` and `admin-repo.hook` 
  
  {% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/quick-start/quick-start-git-event-permissions.png" 
   url="/images/getting-started/quick-start/quick-start-git-event-permissions.png" 
   alt="Scopes for Git runtime token" 
   caption="Scopes for Git runtime token"
   max-width="30%" 
   %}  

##### Git user token for Git-based actions
The Git user token is the user's personal token, used to authenticate every Git-based action of the user in CSDP. You can supply this token during runtime installation, or add it at any time from the CSDP UI.   

  The token must have valid:
  * Expiration date: Default is `30 days`  
  * Scope: `repo`
  
  {% include 
   image.html 
   lightbox="true" 
   file="/images/runtime/git-token-scope-resource-repos.png" 
   url="/images/runtime/git-token-scope-resource-repos.png" 
   alt="Scope for Git personal user token" 
   caption="Scope for Git personal user token"
   max-width="30%" 
   %}    
   
For detailed information on GitHub tokens, see [Creating a personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).  


### What to read next
[Runtime installation]({{site.baseurl}}/docs/runtime/installation/)
