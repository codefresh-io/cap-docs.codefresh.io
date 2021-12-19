---
title: "GitOps approach"
description: ""
group: getting-started
toc: true
---

## GitOps

CSDP is built entirely on the heavily adopted concept of GitOps.

In order to fully understand the benefits of CSDP it is important to first have a short recap of what GitOps is and how it can help.

Lets take a look on the important aspects of GitOps in order to better understand it

* The entire system described declaratively - infrastructure as code <br>
  Infrastructure as code is a modern approach for being able to "declaratively" describe a state of a system as Code while having that single source of truth applied to an end system, in most cases modern cloud native tools. <br>
  Declarative means that configuration is guaranteed by a set of facts instead of by a set of instructions. With your end systems declarations versioned in Git, you have a single source of truth. Your end system can then be easily deployed and rolled back according to the state change in git. And even more importantly, when disaster strikes, your cluster’s infrastructure can also be dependably and quickly reproduced. <br>
  <b>GitOps is just a specific case of infrastructure as code where the end system is a Kubernetes cluster.</b><br>

* The desired system state is versioned in Git <br>
  With the declaration of your system stored in a version control system, and serving as your canonical source of truth, you have a single place from which everything is derived and driven. <br>  
  Besides the ease of use for developers to use the well familiar and convinient approach that they are already using for working on their applicative code, Git also makes complicated tasks like collaboration (via pull requests), security (via signed commits), permissions (repository permissions) and rollback as trivial as it can get.


* Use of dedicated tools to implement transfer of desired state into the end system <br>
  Once the state of your end system is declared and kept under version control, there needs to be a tool and process to apply the updated desired state into the end system. <br>
  One of the used tools for implementing infrastructure as code in the realm of DevOps is [Terraform](https://www.terraform.io/) for example. <br>
  It is totally possible to implement GitOps (infrastructure as code for k8s) using a well battled tool like Terraform which has a plugin system that also supports k8s, but k8s has many nuances that are different than a traditional sync process to a cloud system or some other standard REST api end system. <br>
  To address the specific use cases of k8s, new tools have been created dedicately for implementing GitOps (infrastructure as code for k8s) such as [ArgoCD](https://github.com/argoproj/argo-cd).

  
