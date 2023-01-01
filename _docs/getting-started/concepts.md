---
title: "Concepts in Codefresh"
description: "Understand terminology and nuances in Codefresh"
group: getting-started
toc: true
---

Runtime
A Runtime in Codefresh is a  GitOps installation in your Codefresh account. A Runtime can be Install a Hosted or Hybrid GitOps Runtime. Hosted Runtimes are installed on a Codefresh cluster and managed by Codefresh. Hybrid Runtimes are installed on customer clusters, and managed by the customer.
A single Runtime can connect to and manage multiple remote clusters.
You can install a  single Hosted runtime, and multiple Hybrid Runtines in a Codefresh account. 

Runner
Runner is the hybrid installation option for CI/CD pipelines in your Codefresh account. The Runner allows you to run pipelines on your own Kubernetes cluster, including private clusters behind company firewalls.
Every Runner installation creates a runtime enviroment in your account. You can inst. You can assign the Ru
Assign the Runner to any pipeline to automatically run the pipeline in your own cluster. External integrations (such as Docker registry or Helm repositories) are also available to the Runner making pipelines exactly the same regardless of their runtime environment.
The Runner is installed as a Kubernetes native application on any Kubernetes-compliant cluster and offers the following features:

Access to secure services (such as Git repositories or databases) that are behind the firewall and normally not accessible to the public cloud.
The ability to use special resources in your Codefresh pipeline that are unique to your application (i.e. GPU nodes or other special hardware only present in your data center).
Complete control over the build environment in addition to resources offered to your pipelines.

You can have multiple Runner installations in the same Codefresh account. A Runner can also manage multiple remote clusters across your account. 

Project
A project is a proprietary, top-level entity in Codefresh for grouping pipelines. Pipelines can be grouped according to any criteria that is relevant to your enterprise. The criteria can be logical and based on teams, departments, or location for example, or funtional, and based microservices in applications. 
Goruping pipelines in projects enable centralized viewing and configuration.
Selecting a pipeline shows all pipelines that belong to the same project.
Access control and user-defined variables defined for project are inherited by all the pipelines assigned to the project

There are no limits to the number of projects you can create in your account. You can also create standalone pipelines and assign them later to a project, or detach a pipeline assigned to a project. 


Pipeline
A pipeline is the central component for CI/CD implementation in Codefresh. Everything for CI/CD in Codefresh starts and ends with pipelines, as a pipeline can do only CI, only CD, both CI and CD, or run any custom action, such as unit and integration tests.

A CI pipeline can compile and package code, build and push Docker images. A CD pipeline can deploy applications/artifacts to VMs, Kubernetes clusters, FTP sites, S3 buckets, and more. And a CI/CD pipeline can combine code compilation, integration, and deployment for full CI/CD.




Workflow
A workflow is a new type of Kubernetes resource that lets you define and run automated workflows, and stores their state. 
Argo Workflows is an open source workflow engine that orchestrates parallel tasks on Kubernetes. Argo Workflows is implemented as a set of Kubernetes custom resource definitions (CRDs). 

Argo Workflows is part of the Argo project, which provides Kubernetes-native software delivery tools including Argo CD, Argo Events and Argo Rollouts. 

Codefresh integrates with Argo Workflows to implement continours integration topped with our unique functionlaity on top of vanilla 

Triggers: 

Workflow Templates: Predefined templates  

Steps: 

Workflows dashboard: The Workflows dashboard provides 
See Delivery Pipelines.


Application


Triggers

Events

