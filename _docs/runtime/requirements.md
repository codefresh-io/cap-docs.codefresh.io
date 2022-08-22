---
title: "Hybrid runtime requirements"
description: ""
group: runtime
toc: true
---


The requirements listed are the **_minimum_** requirements to provision **_hybrid runtimes_** in the Codefresh platform.  

> Hosted runtimes are managed by Codefresh. To provision a hosted runtime as part of Hosted GitOps setup, see [Provision a hosted runtime]({{site.baseurl}}/docs/runtime/hosted-runtime/#1-provision-hosted-runtime) in [Set up a hosted (Hosted GitOps) environment]({{site.baseurl}}/docs/runtime/hosted-runtime/).

>In the documentation, Kubernetes and K8s are used interchangeably. 

### Minimum requirements

{: .table .table-bordered .table-hover}
| Item                     | Requirement            |  
| --------------         | --------------           |  
|Kubernetes cluster      | Server version 1.18 and higher, without Argo Project components. {::nomarkdown}<br><b>Tip</b>:  To check the server version, run:<br> <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl version --short</span>.{:/}|
| Ingress controller| Configured on Kubernetes cluster and exposed from the cluster.  {::nomarkdown}<ul><li>Ambassador</li><li>ALB (AWS Application Load Balancer)</li><li><a href="/#nginx-enterprise-configuration">NGINX Enterprise (nginx.org/ingress-controller)</a><br></li><li>NGINX Community (k8s.io/ingress-nginx)</li><li>Istio</li><li>Trafik</li></ul>{:/}. |
|Node requirements| {::nomarkdown}<ul><li>Memory: 5000 MB</li><li>CPU: 2</li></ul>{:/}|
|Runtime namespace | Resource permissions: |
|                  | `ServiceAccount`: Create, Delete         |                             
|                  | `ConfigMap`: Create, Update, Delete |          
|                  | `Service`: Create, Update, Delete |       
|                  | `Role`: In group `rbac.authorization.k8s.io`: Create, Update, Delete |       
|                  |`RoleBinding`: In group `rbac.authorization.k8s.io`: Create, Update, Delete  | 
|                  | `persistentvolumeclaims`: Create, Update, Delete               |   
|                  | `pods`: Create, Update, Delete               | 
| Git providers    |{::nomarkdown}<ul><li>GitHub</li><li>GitLab</li><li>Bitbucket Server</li><!--<li>Bitbucket Cloud</li>--><li>GitHub Enterprise</li></ul>{:/}|
| Git access tokens    | {::nomarkdown}Runtime Git token:<ul><li>Valid expiration date</li><li>Scopes: <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">repo</span> and <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">admin-repo.hook</span></li></ul>Personal access Git token:<ul><li>Valid expiration date</li><li>Scopes: <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">repo</span></li></ul></li></ul>{:/}|

### Ambassador ingress configuration

{: .table .table-bordered .table-hover}
| What to configure    |   When to configure |   
| --------------       | --------------                    | 
|[Verify valid external IP address](#verify-valid-external-ip-address) |   _Before_ installing hybrid runtime  |     
|[Valid SSL certificate](#valid-ssl-certificate) | |
|[TCP support](#tcp-support) |  | 

#### Valid external IP address
Run `kubectl get svc -A` to get a list of services and verify that the `EXTERNAL-IP` column for your ingress controller shows a valid hostname.  
  
#### Valid SSL certificate  
For secure runtime installation, the ingress controller must have a valid SSL certificate from an authorized CA (Certificate Authority).  

#### TCP support  
Configure to handle TCP requests.  

### ALB AWS configuration

{: .table .table-bordered .table-hover}
| What to configure    |   When to configure |   
| --------------       | --------------                    | 
|[Verify valid external IP address](#verify-valid-external-ip-address) |   _Before_ installing hybrid runtime  |     
|[Valid SSL certificate](#valid-ssl-certificate) | |
|[TCP support](#tcp-support) |  | 
|[spec.controller](#spec-controller) |  | 

#### Valid external IP address
Run `kubectl get svc -A` to get a list of services and verify that the `EXTERNAL-IP` column for your ingress controller shows a valid hostname.  
  
#### Valid SSL certificate  
For secure runtime installation, the ingress controller must have a valid SSL certificate from an authorized CA (Certificate Authority).  

#### TCP support  
Configure to handle TCP requests.  

#### spec.controller 
In the ingress resource file, verify that `spec.controller` is configured as `ingress.k8s.aws/alb`. 

```yaml
apiVersion: networking.k8s.io/v1
kind: IngressClass
metadata:
  name: alb
spec:
  controller: ingress.k8s.aws/alb
```
#### Alias DNS record in route53 to load balancer

Make sure you have a DNS record available in the correct hosted zone.  
After the hybrid runtime completes installation, a load balancer is created. You should now create an `Alias` record in Amazon Route 53, and map your zone apex (`example.com`) DNS name to your Amazon CloudFront distribution.
For more information, see [Creating records by using the Amazon Route 53 console](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-creating.html){:target="\_blank"}.

{% include image.html
  lightbox="true"
  file="/images/runtime/post-install-alb-ingress.png"
  url="/images/runtime/post-install-alb-ingress.png"
  alt="Route 53 record settings for AWS ALB"
  caption="Route 53 record settings for AWS ALB"
  max-width="60%"
%}

#### Git integration registration
If the installation failed, as could happen if the DNS record was created, manually create and register Git integrations using these commands:  

  `cf integration git add default --runtime <RUNTIME-NAME> --api-url <API-URL>`  
  
  `cf integration git register default --runtime <RUNTIME-NAME> --token <RUNTIME-AUTHENTICATION-TOKEN>`  
  where:  


### NGINX Enterprise configuration
For general information, see [NGINX ingress controller documentation](https://docs.nginx.com/nginx-ingress-controller){:target="\_blank}. 

{: .table .table-bordered .table-hover}
| What to configure    |   When to configure |   
| --------------       | --------------                    | 
|[Verify valid external IP address](#verify-valid-external-ip-address) |   _Before_ installing hybrid runtime  |     
|[Valid SSL certificate](#valid-ssl-certificate) | |
|[TCP support](#tcp-support) |  | 
|[Report status](#report-status) |  | 
|[Patch certificate secret](#verify-valid-external-ip-address) |  _After_ installing hybrid runtime  

#### Valid external IP address
Run `kubectl get svc -A` to get a list of services and verify that the `EXTERNAL-IP` column for your ingress controller shows a valid hostname.  
  
#### Valid SSL certificate  
For secure runtime installation, the ingress controller must have a valid SSL certificate from an authorized CA (Certificate Authority).  

#### TCP support  
Configure to handle TCP requests.  


#### NGINX Ingress: Enable report status to cluster

If the ingress controller is not configured to report its status to the cluster, Argo’s health check reports the health status as “progressing” resulting in a timeout error during installation.  

* Pass `--report-ingress-status` to `deployment`.

    ```yaml
    spec:                                                                                                                                                                 
      containers: 
       - args:                                                                                                                                              
        - -report-ingress-status
    ```

#### NGINX Ingress Operator: Enable report status to cluster

If the ingress controller is not configured to report its status to the cluster, Argo’s health check reports the health status as “progressing” resulting in a timeout error during installation.  

1. Add this to the `Nginxingresscontrollers` resource file:

   ```yaml
   ...
   spec:
     reportIngressStatus:
       enable: true
   ...
  ```

1. Make sure you have a certificate secret in the same namespace as the runtime. Copy an existing secret if you don't have one.  
You will need to add this to the `ingress-master` when you have completed runtime installation.

#### NGINX Ingress Operator: Patch certificate secret
>  The certifcate secret must be configured _after_ installing the hybrid runtime.

Patch the certificate secret in `spec.tls` of the `ingress-master` resource.  
The secret must be in the same namespace as the runtime.

1. Go to the runtime namespace with the NGINX ingress controller.
1. In `ingress-master`, add to `spec.tls`:  

    ```yaml
    tls:                                                                                                                                                                    
     - hosts:                                                                                                                                                                
     - <host_name>                                                                                             
     secretName: <secret_name>
   ```




### NGINX Community version configuration

Codefresh is supported with and has been tested with major providers. For your convenience, we have provided configuration instructions, both for supported and untested providers in [Provider-specific configuration](#provider-specific-configuration).  


In addition, make sure your NGINX community ingress controller is  must be configured as

{: .table .table-bordered .table-hover}
| What to configure    |   When to configure |   
| --------------       | --------------                    | 
|[Verify valid external IP address](#verify-valid-external-ip-address) |   _Before_ installing hybrid runtime  |     
|[Valid SSL certificate](#valid-ssl-certificate) | |
|[TCP support](#tcp-support) |  | 
|[Report status](#report-status) |  | 


#### Valid external IP address
Run `kubectl get svc -A` to get a list of services, and verify that the `EXTERNAL-IP` column for your ingress controller shows a valid hostname.  
  
#### Valid SSL certificate  
For secure runtime installation, the ingress controller must have a valid SSL certificate from an authorized CA (Certificate Authority).  

#### TCP support  
Configure to handle TCP requests.  

Here's an example of TCP configuration for NGINX Community on AWS.  
Verify that the `ingress-nginx-controller` service manifest has either of the following annotations:  

`service.beta.kubernetes.io/aws-load-balancer-backend-protocol: "tcp"`  
OR  
`service.beta.kubernetes.io/aws-load-balancer-type: nlb` 

#### Provider-specific configuration

> The instructions are valid for `k8s.io/ingress-nginx`, the community version of NGINX.

<details>
<summary><b>AWS</b></summary>
<ol>
<li>Apply:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/aws/deploy.yaml</span>
</li>
<li>Verify a valid external address exists:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl get svc ingress-nginx-controller -n ingress-nginx</span>
</li>
</ol>
For additional configuration options, see <a target="_blank" href="https://kubernetes.github.io/ingress-nginx/deploy/#aws">ingress-nginx documentation for AWS</a>.
</details>
<details>
<summary><b>Azure (AKS)</b></summary>
<ol>
<li>Apply:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/cloud/deploy.yaml</span>
</li>
<li>Verify a valid external address exists:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl get svc ingress-nginx-controller -n ingress-nginx</span>
</li>
</ol>
For additional configuration options, see <a target="_blank" href="https://docs.microsoft.com/en-us/azure/aks/ingress-internal-ip?tabs=azure-cli#create-an-ingress-controller">ingress-nginx documentation for AKS</a>.

</details>

<details>
<summary><b>Bare Metal Clusters</b></summary>
<ol>
<li>Apply:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/baremetal/deploy.yaml</span>
</li>
<li>Verify a valid external address exists:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl get svc ingress-nginx-controller -n ingress-nginx</span>
</li>
</ol>
Bare-metal clusters often have additional considerations. See <a target="_blank" href="https://kubernetes.github.io/ingress-nginx/deploy/baremetal/">Bare-metal ingress-nginx considerations</a>.

</details>

<details>
<summary><b>Digital Ocean</b></summary>
<ol>
<li>Apply:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/do/deploy.yaml</span>
</li>
<li>Verify a valid external address exists:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl get svc ingress-nginx-controller -n ingress-nginx</span>
</li>
</ol>
For additional configuration options, see <a target="_blank" href="https://kubernetes.github.io/ingress-nginx/deploy/#digital-ocean">ingress-nginx documentation for Digital Ocean</a>.

</details>

<details>
<summary><b>Docker Desktop</b></summary>
<ol>
<li>Apply:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/cloud/deploy.yaml</span>
</li>
<li>Verify a valid external address exists:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl get svc ingress-nginx-controller -n ingress-nginx</span>
</li>
</ol>
For additional configuration options, see <a target="_blank" href="https://kubernetes.github.io/ingress-nginx/deploy/#docker-desktop">ingress-nginx documentation for Docker Desktop</a>.<br>
<b>Note:</b> By default, Docker Desktop services will provision with localhost as their external address. Triggers in delivery pipelines cannot reach this instance unless they originate from the same machine where Docker Desktop is being used.

</details>

<details>
<summary><b>Exoscale</b></summary>
<ol>
<li>Apply:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/exoscale/deploy.yaml</span>
</li>
<li>Verify a valid external address exists:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl get svc ingress-nginx-controller -n ingress-nginx</span>
</li>
</ol>
For additional configuration options, see <a target="_blank" href="https://github.com/exoscale/exoscale-cloud-controller-manager/blob/master/docs/service-loadbalancer.md">ingress-nginx documentation for Exoscale</a>.

</details>


<details>
<summary><b>Google (GKE)</b></summary>
<br>
<b>Add firewall rules</b>
<br>
GKE by default limits outbound requests from nodes. For the runtime to communicate with the control-plane in Codefresh, add a firewall-specific rule.

<ol>
<li>Find your cluster's network:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">gcloud container clusters describe [CLUSTER_NAME] --format=get"(network)"</span>
</li>
<li>Get the Cluster IPV4 CIDR:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">gcloud container clusters describe [CLUSTER_NAME] --format=get"(clusterIpv4Cidr)"</span>
</li>
<li>Replace the `[CLUSTER_NAME]`, `[NETWORK]`, and `[CLUSTER_IPV4_CIDR]`, with the relevant values: <br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">gcloud compute firewall-rules create "[CLUSTER_NAME]-to-all-vms-on-network" </span><br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">  
    --network="[NETWORK]" \
    </span><br>
   <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">  
    --source-ranges="[CLUSTER_IPV4_CIDR]" \
    </span><br>
   <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">  
   --allow=tcp,udp,icmp,esp,ah,sctp
    </span><br>
</li> 
</ol>
<br>
<b>Use ingress-nginx</b><br>
<ol>
  <li>Create a `cluster-admin` role binding:<br>
      <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">  
        kubectl create clusterrolebinding cluster-admin-binding \
      </span><br>
      <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">  
        --clusterrole cluster-admin \
      </span><br>
      <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">  
        --user $(gcloud config get-value account)
      </span><br>
  </li>
  <li>Apply:<br>
      <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">  
        kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/cloud/deploy.yaml
      </span>
  </li>
  <li>Verify a valid external address exists:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl get svc ingress-nginx-controller -n ingress-nginx</span>
  </li>

</ol>
We recommend reviewing the <a target="_blank" href="https://kubernetes.github.io/ingress-nginx/deploy/#gce-gke">provider-specific documentation for GKE</a>.

</details>


<details>
<summary><b>MicroK8s</b></summary>
<ol>
<li>Install using Microk8s addon system:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">microk8s enable ingress</span>
</li>
<li>Verify a valid external address exists:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl get svc ingress-nginx-controller -n ingress-nginx</span>
</li>
</ol>
MicroK8s has not been tested with Codefresh, and may require additional configuration. For details, see <a target="_blank" href="https://microk8s.io/docs/addon-ingress">Ingress addon documentation</a>.

</details>


<details>
<summary><b>MiniKube</b></summary>
<ol>
<li>Install using MiniKube addon system:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">minikube addons enable ingress</span>
</li>
<li>Verify a valid external address exists:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl get svc ingress-nginx-controller -n ingress-nginx</span>
</li>
</ol>
MiniKube has not been tested with Codefresh, and may require additional configuration. For details, see <a target="_blank" href="https://kubernetes.github.io/ingress-nginx/deploy/#minikube">Ingress addon documentation</a>.

</details>



<details>
<summary><b>Oracle Cloud Infrastructure</b></summary>
<ol>
<li>Apply:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/cloud/deploy.yaml</span>
</li>
<li>Verify a valid external address exists:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl get svc ingress-nginx-controller -n ingress-nginx</span>
</li>
</ol>
For additional configuration options, see <a target="_blank" href="https://kubernetes.github.io/ingress-nginx/deploy/#oracle-cloud-infrastructure">ingress-nginx documentation for Oracle Cloud</a>.

</details>

<details>
<summary><b>Scaleway</b></summary>
<ol>
<li>Apply:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/scw/deploy.yaml</span>
</li>
<li>Verify a valid external address exists:<br>
    <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl get svc ingress-nginx-controller -n ingress-nginx</span>
</li>
</ol>
For additional configuration options, see <a target="_blank" href="https://kubernetes.github.io/ingress-nginx/deploy/#scaleway">ingress-nginx documentation for Scaleway</a>.

</details> 

### Istio ingress controller configuration
For general information, see [NGINX ingress controller documentation](https://docs.nginx.com/nginx-ingress-controller){:target="\_blank}. 

{: .table .table-bordered .table-hover}
| What to configure    |   When to configure |   
| --------------       | --------------                    | 
|[Valid external IP address](#verify-valid-external-ip-address) |   _Before_ installing hybrid runtime  |     
|[Valid SSL certificate](#valid-ssl-certificate) | |
|[TCP support](#tcp-support) |  | 
|[Cluster routing service](#cluster-routing-service) | _After_ installing hybrid runtime | 

#### Valid external IP address
Run `kubectl get svc -A` to get a list of services and verify that the `EXTERNAL-IP` column for your ingress controller shows a valid hostname.  
  
#### Valid SSL certificate  
For secure runtime installation, the ingress controller must have a valid SSL certificate from an authorized CA (Certificate Authority).  

#### TCP support  
Configure to handle TCP requests.  

#### Cluster routing service
Configure the `VirtualService` to route traffic to the `app-proxy` and `webhook` services, as in the examples below.  

**`VirtualService` example for `app-proxy`:** 

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  namespace: test-runtime3 # replace with your runtime name
  name: cap-app-proxy 
spec:
  hosts:
    - my.support.cf-cd.com # replace with your host name
  gateways:
    - my-gateway
  http:
    - match:
      - uri:
          prefix: /app-proxy 
      route:
      - destination:
          host: cap-app-proxy 
          port:
            number: 3017
```
**`VirtualService` example for `webhook`:** 

```yaml  
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  namespace: test-runtime3 # replace with your runtime name
  name: csdp-default-git-source
spec:
  hosts:
    - my.support.cf-cd.com # replace with your host name
  gateways:
    - my-gateway
  http:
    - match:
      - uri:
          prefix: /webhooks/test-runtime3/push-github # replace `test-runtime3` with your runtime name
      route:
      - destination:
          host: push-github-eventsource-svc 
          port:
            number: 80
```

### Traefik ingress controller configuration
For general information, see [NGINX ingress controller documentation](https://docs.nginx.com/nginx-ingress-controller){:target="\_blank}. 

{: .table .table-bordered .table-hover}
| What to configure    |   When to configure |   
| --------------       | --------------                    | 
|[Valid external IP address](#verify-valid-external-ip-address) |   _Before_ installing hybrid runtime  |     
|[Valid SSL certificate](#valid-ssl-certificate) | |
|[TCP support](#tcp-support) |  | 
|[Enable report status](#tcp-support) |  | 
|[Cluster routing service](#cluster-routing-service) | _After_ installing hybrid runtime | 

#### Valid external IP address
Run `kubectl get svc -A` to get a list of services and verify that the `EXTERNAL-IP` column for your ingress controller shows a valid hostname.  
  
#### Valid SSL certificate  
For secure runtime installation, the ingress controller must have a valid SSL certificate from an authorized CA (Certificate Authority).  

#### TCP support  
Configure to handle TCP requests.  

#### Report status to cluster  
By default, the Traefik ingress controller is not configured to report its status to the cluster.  If not configured,  Argo’s health check reports the health status as “progressing”, resulting in a timeout error during installation.  

To enable reporting its status, add `publishedService` to `providers.kubernetesIngress.ingressEndpoint`.  
  
The value must be in the format `"<namespace>/<service-name>"`, where:  
  `<service-name>` is the Traefik service from which to copy the status

```yaml
...
providers:
  kubernetesIngress:
    ingressEndpoint:
      publishedService: "<namespace>/<traefik-service>" # Example, "codefresh/traefik-default" 
...
```

### What to read next
[Installing hybrid runtimes]({{site.baseurl}}/docs/runtime/installation/)
