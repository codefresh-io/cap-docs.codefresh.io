---
title: "Runtime installation and configuration"
description: ""
group: runtime
toc: true
---


Codefresh runtime installs Codefresh Software Development Platoform (CSDP), comprising Argo CD ecosystem components and Codefresh-specific components. The Argo CD components are derieved from a conformed fork.

There are two parts to installing runtimes:
1. Installing the Codefresh CLI, a one-time action, typically required only for initial CDSP setup.
2. Installing the Codefresh runtime from the CLI. You can install and provision as many runtimes as you need to. Every runtime is installed at the cluster-level.

Verify [system requirements]({{site.baseurl}}/docs/runtime/monitor-manage-runtimes)


### Where do you install runtimes?
* If this is the first runtime you are installing, on the Welcome page, select **+ Install Runtime**.
* To install additional runtimes, select **Account Settings**, and then from the sidebar, select **Configuration > Runtimes**. Then from the top-right, select **+ Add Runtime**.

### Installing the Codefresh CLI
* Codefresh CLI mode  
  Install the Codefresh CLI using the option that best suits you: Curl, Brew, or standard download. If you are not sure which OS to select for Curl and Brew, simply select one, and Codefresh automatically identifies and selects the right OS for CLI installation.


  
### Installing the Codefresh runtime

#### Runtime components
 CSDP  create will create a repo (if not exist) and bootstrap (if needed) a new installation, levering the argocd-autopilot. The CLI will determine whether the new runtime is an overlay of an existing runtime or a completely new runtime.

 
**Argo CD components**

* Two Github repositories, one for the runtime’s infrastructure, and one for the first GitOps source defined through the default Git Source entity
* Project, comprising an Argo CD AppProject and an ApplicationSet
* Installations of the following applications in the project:
  * Argo CD 
  * Argo Workflows 
  * Argo Events
  * Argo Rollouts
  
**Codefresh-specific components**
* Codefresh Applications in the Argo CD AppProject:  
  * ArgoCD Agent that feeds GitOps data into CSDP
  * App-proxy facilitating behind-firewall access to Git 
  * Git Source entity that references a specific directory on a different repository
*  A Service that pulls the changes from the repository defined in the Git Source, analyses them, and generates events for the created/updated/deleted files to CSDP
*  Event Sources and Sensors for the K8s cluster, Git Sources, and CSDP 
   (NIMA: should we detail as below?
  * An EventSource and Sensor that sets a WebHook on the Git source, and triggers the above service
  *  An EventSource and Sensor that registers to events on the K8s cluster, and reports them to CSDP
  *  Components-reporter, an EventSource and Sensor that listens to Codefresh components and reports events to CSDP)

#### Runtime prerequisites
Before you install a runtime, make sure you have the following: 

* **Ingress controller in Kubernetes cluster** 
  Configure your Kubernetes cluster with an Ingress controller component, that is exposed from the cluster. (NIMA: Should we add what CF supports? Do we have limitations?) For detailed information, see the [Kubernetes documentation on ingress controllers](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/).
  Note down the external address of the Ingress controller, as you will need this when you install the runtime. 

* **Default `IngressClass`** 
  If you do not have any `IngressClass` resource tagged as the default, add this `ingressclass.kubernetes.io/is-default-class` annotation with the value of `true` to the resource.
  
  ```yaml
  apiVersion: networking.k8s.io/v1
  kind: IngressClass
  metadata:
  annotations:
    ingressclass.kubernetes.io/is-default-class: "true" 
  ```

* **Git tokens**  
  You need a personal access Git token for authentication to the Git installation repo, that you will create or select during runtime installation.   
  To create a Git token, see [Creating a personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

  > When you create the Git token, set the correct expiration date and scope: 
   Expiration: (NIMA: Itai to add)  
   Scope: `repo`

#### Runtime installation mandatory flags
To install the runtime from the CLI, you need to pass two mandatory flags. All other flags are optional.  
`--git-token`: The Git token authenticating access to the Git installation repository defined.  
`--ingress-host`: The IP address/host name/FQDN of the ingress controller component.
 
Once the runtime is successfully installed, it is provisioned on the Kubernetes cluster, and displayed on the Runtimes page. 

### What to read next
[Monitor and manage runtimes]({{site.baseurl}}/docs/runtime/monitor-manage-runtimes/)
[Manage Git Sources]({{site.baseurl}}/docs/runtime/git-sources/)
