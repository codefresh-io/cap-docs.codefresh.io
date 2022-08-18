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
|Kubernetes cluster      |  Server version 1.18 and higher, without Argo Project components. Tip:  To check the server version, run `kubectl version --short`.|
| Ingress controller| Configured on Kubernetes cluster and exposed from the cluster.  {::nomarkdown} </br><ul><li>Ambassador</li><li>ALB (AWS Application Load Balancer)</li><li>NGINX Enterprise (nginx.org/ingress-controller)<br></li><li>NGINX Community (k8s.io/ingress-nginx)</li><li>Istio</li><li>Trafik</li></ul>{:/}. |
|Node requirements| {::nomarkdown}<ul><li>Memory: 5000 MB</li><li>CPU: 2</li></ul>{:/}|
|Runtime namespace | Resource permissions: |
|                  | `ServiceAccount`: Create, Delete         |                             
|                  | `ConfigMap`: Create, Update, Delete |          
|                  | `Service`: Create, Update, Delete |       
|                  | `Role`: In group `rbac.authorization.k8s.io`: Create, Update, Delete |       
|                  |`RoleBinding`: In group `rbac.authorization.k8s.io`: Create, Update, Delete  | 
|                  | `persistentvolumeclaims`: Create, Update, Delete               |   
|                  | `pods`: Create, Update, Delete               | 
| Git providers    |{::nomarkdown}Hosted: <ul><li>GitHub</li></ul>Hybrid:<ul><li>GitHub</li><li>GitLab</li>Bitbucket Server</li><li>GitHub Enterprise</li></ul>{:/}|
| Git access tokens    | {::nomarkdown}Runtime Git token:<ul><li>Valid expiration date</li><li>Scopes: `repo` and `admin-repo.hook`</li></ul>Personal access Git token:<ul><li>Valid expiration date</li><li>Scopes: `repo` and `admin-repo.hook`</li></ul></li></ul>{:/}|

### NGINX Enterprise configuration
For general information, see [NGINX ingress controller documentation](https://docs.nginx.com/nginx-ingress-controller){:target="\_blank}. 

{: .table .table-bordered .table-hover}
| What to configure    |   When to configure |   
| --------------       | --------------                    | 
| [Verify valid external IP address](#verify-valid-external-ip-address) |   _Before_ installing hybrid runtime  |     
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

Here's an example of TCP configuration for NGINX on AWS.  
Verify that the ingress-nginx-controller service manifest has either of the following annotations:  

`service.beta.kubernetes.io/aws-load-balancer-backend-protocol: "tcp"`  
OR  
`service.beta.kubernetes.io/aws-load-balancer-type: nlb`  

#### NGINX Ingress: Enable report status to cluster
* Pass the `- -report-ingress-status` to `deployment`.

    ```yaml
    spec:                                                                                                                                                                 
      containers: 
       - args:                                                                                                                                              
       - -report-ingress-status
    ```

#### NGINX Ingress Operator: Enable report status to cluster

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
> This configuration must be completed _after_ installing the hybrid runtime.

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


### What to read next
[Installing hybrid runtimes]({{site.baseurl}}/docs/runtime/installation/)
