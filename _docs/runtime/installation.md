---
title: "Runtime installation and configuration"
description: ""
group: runtime
toc: true
---


Codefresh runtime installs the Codefresh Software Development Platoform (CSDP), comprising Argo CD ecosystem components and Codefresh-specific components. The Argo CD components are derived from a fork of the Argo ecosystem.

There are two parts to installing runtimes:
1. Installing the Codefresh CLI, a one-time action, typically required only for initial CDSP setup.
2. Installing the Codefresh runtime from the CLI. You can install and provision as many runtimes as you need to. A runtime is installed in a specific namespace on your cluster. You can install more runtimes on different clusters in your deployment.

Before installation, review [CSDP architecture]({{site.baseurl}}/docs/getting-started/architecture)], and also verify [system requirements]({{site.baseurl}}/docs/runtime/monitor-manage-runtimes).


### Where do you install runtimes?
* If this is your first CSDP installation, in the Welcome page, select **+ Install Runtime**.
* To install additional runtimes, select **Account Settings**, and then from the sidebar, select **Configuration > Runtimes**. From the top-right, select **+ Add Runtime**.

### Installing the Codefresh CLI
* Codefresh CLI mode  
  Install the Codefresh CLI using the option that best suits you: Curl, Brew, or standard download. If you are not sure which OS to select for Curl and Brew, simply select one, and Codefresh automatically identifies and selects the right OS for CLI installation

### Installing the Codefresh runtime

#### Runtime components
 
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
  * App-proxy facilitating behind-firewall access to Git 
  * Git Source entity that references a specific directory on a different repository
   

#### Runtime prerequisites
Before you install a runtime, make sure you have the following: 

* **Ingress controller in Kubernetes cluster** 
  Configure your Kubernetes cluster with an Ingress controller component, that is exposed from the cluster. For detailed information, see the [Kubernetes documentation on ingress controllers](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/).
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
  For GitHub, you need a Personal Access Token (PAT) for authentication to the Git installation repo, that you will create or select during runtime installation.   
  To create a Git token, see [Creating a personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

  > When you create the Git token, set the correct expiration date and scope: 
   Expiration: Default is `30 days`  
   Scope: `repo`

#### Runtime installation mandatory flags
To install the runtime from the CLI, you need to pass two mandatory flags. All other flags are optional.  
`--git-token`: The Git token authenticating access to the Git installation repository defined.  
`--ingress-host`: The IP address/host name/FQDN of the ingress controller component.
 
Once the runtime is successfully installed, it is provisioned on the Kubernetes cluster, and displayed on the Runtimes page. 

### What to read next
[Manage runtimes]({{site.baseurl}}/docs/runtime/monitor-manage-runtimes/)
[Manage Git Sources]({{site.baseurl}}/docs/runtime/git-sources/)
