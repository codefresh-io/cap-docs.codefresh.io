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
| Ingress controller| Configured on Kubernetes cluster and exposed from the cluster. {::nomarkdown} <br>Supported and tested ingress controllers include: <ul><li>Ambassador</li>{:/}(see [Ambassador ingress configuration](#ambassador-ingress-configuration)){::nomarkdown}<li>AWS ALB (Application Load Balancer)</li>{:/} (see [AWS ALB ingress configuration](#aws-alb-ingress-configuration)){::nomarkdown}<li>Istio</li>{:/} (see [Istio ingress configuration](#istio-ingress-configuration)){::nomarkdown}<li>NGINX Enterprise (nginx.org/ingress-controller)</li>{:/} (see [NGINX Enterprise ingress configuration](#nginx-enterprise-ingress-configuration)){::nomarkdown}<li>NGINX Community (k8s.io/ingress-nginx)</li> {:/} (see [NGINX Community ingress configuration](#nginx-community-version-ingress-configuration)){::nomarkdown}<li>Trafik</li>{:/}(see [Traefik ingress configuration](#traefik-ingress-configuration))|
|Gateway API| Namespace with the Codefresh  in `allowedRoutes.namespaces` |
|Node requirements| {::nomarkdown}<ul><li>Memory: 5000 MB</li><li>CPU: 2</li></ul>{:/}|
|Cluster permissions | Cluster admin permissions |
|Git providers    |{::nomarkdown}<ul><li>GitHub</li><!--<li>GitLab</li><li>Bitbucket Server</li><li>Bitbucket Cloud</li><li>GitHub Enterprise</li>--></ul>{:/}|
|Git access tokens    | {::nomarkdown}Runtime Git token:<ul><li>Valid expiration date</li><li>Scopes: <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">repo</span> and <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">admin-repo.hook</span></li></ul>Personal access Git token:<ul><li>Valid expiration date</li><li>Scopes: <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">repo</span></li></ul></li></ul>{:/}|

### Ambassador ingress configuration
For detailed configuration information, see the [Ambassador ingress controller documentation](https://www.getambassador.io/docs/edge-stack/latest/topics/running/ingress-controller){:target="\_blank"}.  

The table below lists the specific configuration requirements for Codefresh.

{: .table .table-bordered .table-hover}
| What to configure    |   When to configure |   
| --------------       | --------------                    | 
|Valid external IP address |   _Before_ installing hybrid runtime  |     
|Valid SSL certificate | |
|TCP support|  | 

#### Valid external IP address
Run `kubectl get svc -A` to get a list of services and verify that the `EXTERNAL-IP` column for your ingress controller shows a valid hostname.  
  
#### Valid TLS certificate  
For secure runtime installation, the ingress controller must have a valid TLS certificate.  
> Use the FQDN (Fully Qualified Domain Name) of the ingress controller for the TLS certificate.

#### TCP support  
Configure the ingress controller to handle TCP requests.  

### AWS ALB ingress configuration

For detailed configuration information, see the [ALB AWS ingress controller documentation](https://kubernetes-sigs.github.io/aws-load-balancer-controller/v2.4){:target="\_blank"}.  

The table below lists the specific configuration requirements for Codefresh.

{: .table .table-bordered .table-hover}
| What to configure    |   When to configure |   
| --------------       | --------------                    | 
|Valid external IP address |   _Before_ installing hybrid runtime  |     
|Valid SSL certificate | |
|TCP support|  |  
|Controller  configuration] |  | 
|Alias DNS record in route53 to load balancer | _After_ installing hybrid runtime | 
|(Optional) Git integration registration | | 


#### Valid external IP address
Run `kubectl get svc -A` to get a list of services and verify that the `EXTERNAL-IP` column for your ingress controller shows a valid hostname.  
  
#### Valid TLS certificate  
For secure runtime installation, the ingress controller must have a valid TLS certificate.  
> Use the FQDN (Fully Qualified Domain Name) of the ingress controller for the TLS certificate.

#### TCP support  
Configure the ingress controller to handle TCP requests.  

#### Controller configuration
In the ingress resource file, verify that `spec.controller` is configured as `ingress.k8s.aws/alb`. 

```yaml
apiVersion: networking.k8s.io/v1
kind: IngressClass
metadata:
  name: alb
spec:
  controller: ingress.k8s.aws/alb
```
#### Create an alias to load balancer in route53

>  The alias  must be configured _after_ installing the hybrid runtime.

1. Make sure a DNS record is available in the correct hosted zone. 
1. _After_ hybrid runtime installation, in Amazon Route 53, create an alias to route traffic to the load balancer that is automatically created during the installation:  
  * **Record name**: Enter the same record name used in the installation.
  * Toggle **Alias** to **ON**.
  * From the **Route traffic to** list, select **Alias to Application and Classic Load Balancer**.
  * From the list of Regions, select the region. For example, **US East**.
  * From the list of load balancers, select the load balancer that was created during installation.  

For more information, see [Creating records by using the Amazon Route 53 console](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-creating.html){:target="\_blank"}.

{% include image.html
  lightbox="true"
  file="/images/runtime/post-install-alb-ingress.png"
  url="/images/runtime/post-install-alb-ingress.png"
  alt="Route 53 record settings for AWS ALB"
  caption="Route 53 record settings for AWS ALB"
  max-width="60%"
%}

#### (Optional) Git integration registration
If the installation failed, as can happen if the DNS record was not created within the timeframe, manually create and register Git integrations using these commands:  

  `cf integration git add default --runtime <RUNTIME-NAME> --api-url <API-URL>`  
  
  `cf integration git register default --runtime <RUNTIME-NAME> --token <RUNTIME-AUTHENTICATION-TOKEN>`  
 

### Istio ingress configuration
For detailed configuration information, see [Istio ingress controller documentation](https://istio.io/latest/docs/tasks/traffic-management/ingress/kubernetes-ingress){:target="\_blank}.  

The table below lists the specific configuration requirements for Codefresh.

{: .table .table-bordered .table-hover}
| What to configure    |   When to configure |   
| --------------       | --------------   | 
|Valid external IP address |_Before_ installing hybrid runtime  |     
|Valid SSL certificate| |
|TCP support |  | 
|Cluster routing service | _After_ installing hybrid runtime | 

#### Valid external IP address
Run `kubectl get svc -A` to get a list of services and verify that the `EXTERNAL-IP` column for your ingress controller shows a valid hostname.  
  
#### Valid TLS certificate  
For secure runtime installation, the ingress controller must have a valid TLS certificate.  
> Use the FQDN (Fully Qualified Domain Name) of the ingress controller for the TLS certificate.

#### TCP support  
Configure the ingress controller to handle TCP requests.  

#### Cluster routing service
>  The cluster routing service must be configured _after_ installing the hybrid runtime.

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

### NGINX Enterprise ingress configuration

For detailed configuration information, see [NGINX ingress controller documentation](https://docs.nginx.com/nginx-ingress-controller){:target="\_blank}.  

The table below lists the specific configuration requirements for Codefresh.

{: .table .table-bordered .table-hover}
| What to configure    |   When to configure |   
| --------------       | --------------                    | 
|Verify valid external IP address |_Before_ installing hybrid runtime  |     
|Valid SSL certificate | |
|TCP support|  | 
|NGINX Ingress: Enable report status to cluster |  | 
|NGINX Ingress Operator: Enable report status to cluster| |
|Patch certificate secret |_After_ installing hybrid runtime  

#### Valid external IP address
Run `kubectl get svc -A` to get a list of services and verify that the `EXTERNAL-IP` column for your ingress controller shows a valid hostname.  
  
#### Valid TLS certificate  
For secure runtime installation, the ingress controller must have a valid TLS certificate.  
> Use the FQDN (Fully Qualified Domain Name) of the ingress controller for the TLS certificate.

#### TCP support  
Configure the ingress controller to handle TCP requests.   


#### NGINX Ingress: Enable report status to cluster

If the ingress controller is not configured to report its status to the cluster, Argo’s health check reports the health status as “progressing” resulting in a timeout error during installation.  

* Pass `--report-ingress-status` to `deployment`.

```yaml
spec:                                                                                                                                                                 
  containers: 
    - args:                                                                                                                                              
      - --report-ingress-status
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

#### Patch certificate secret
>  The certificate secret must be configured _after_ installing the hybrid runtime.

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


### NGINX Community version ingress configuration

Codefresh has been tested with and supports implementations of the major providers. For your convenience, we have provided configuration instructions, both for supported and untested providers in [Provider-specific configuration](#provider-specific-configuration).  


The table below lists the specific configuration requirements for Codefresh.

{: .table .table-bordered .table-hover}
| What to configure    |   When to configure |   
| --------------       | --------------                    | 
|Verify valid external IP address |   _Before_ installing hybrid runtime  |     
|Valid SSL certificate | |
|TCP support |  | 


#### Valid external IP address
Run `kubectl get svc -A` to get a list of services, and verify that the `EXTERNAL-IP` column for your ingress controller shows a valid hostname.  
  
#### Valid TLS certificate  
For secure runtime installation, the ingress controller must have a valid TLS certificate.  
> Use the FQDN (Fully Qualified Domain Name) of the ingress controller for the TLS certificate.

#### TCP support  
Configure the ingress controller to handle TCP requests.   

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



### Traefik ingress configuration
For detailed configuration information, see [Traefik ingress controller documentation](https://doc.traefik.io/traefik/providers/kubernetes-ingress){:target="\_blank}.  

The table below lists the specific configuration requirements for Codefresh.

{: .table .table-bordered .table-hover}

| What to configure    |   When to configure |   
| --------------       | --------------  | 
|Valid external IP address | _Before_ installing hybrid runtime  |     
|Valid SSL certificate | |
|TCP support |  | 
|Enable report status to cluster|  | 

#### Valid external IP address
Run `kubectl get svc -A` to get a list of services and verify that the `EXTERNAL-IP` column for your ingress controller shows a valid hostname.  
  
#### Valid TLS certificate  
For secure runtime installation, the ingress controller must have a valid TLS certificate.  
> Use the FQDN (Fully Qualified Domain Name) of the ingress controller for the TLS certificate.

#### TCP support  
Configure the ingress controller to handle TCP requests.   

#### Enable report status to cluster 
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
[Hybrid runtime installation flags]({{site.baseurl}}/docs/runtime/installation//#hybrid-runtime-installation-flags)  
[Install hybrid runtimes]({{site.baseurl}}/docs/runtime/installation/)
