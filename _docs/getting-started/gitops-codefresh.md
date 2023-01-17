---
title: "WIP: Codefresh for GitOps"
description: "GitOps and Argo CD with Codefresh"
group: getting-started
toc: true
---

This article reviews the concepts of GitOps and Argo CD, and how Codefresh integrates with and impe


## What is GitOps?
Weaveworks, who introduced the concept of GitOps, describes it as "a way of implementing Continuous Deployment for cloud native applications. It focuses on a developer-centric experience when operating infrastructure, by using tools developers are already familiar with, including Git and Continuous Deployment tools."

At the root of the GitOps pardigm is having "Git as the single source of truth". Git repositories are the source-control systems that declaratively describe  applications and infrastructure using code. The continuous integration and continuous delivery processes synchronize these changes with live environments, making sure that the production state always match the desried in Git.

The key difference is that while the traditional workflow is based on “pushing” new code changes through the pipeline to production, a GitOps workflow is a “pull” process in which new changes are submitted, and the GitOps agent detects and synchronizes them with the production environment.

Read more in our blog, [What is a GitOps workflow](https://codefresh.io/learn/gitops/gitops-workflow-vs-traditional-workflow-what-is-the-difference/){:target="\_blank"}.


## Argo CD

Argo CD is "a declarative, GitOps continuous delivery tool for Kubernetes."

Argo CD uses a Kubernetes controller to constantly monitor the state of all resources in production and compare them against the desired states set in Git. When then are changes to the desired state, the controller detects and works to synchronize the production to the desired configuration. 

Argo CD is also designed to work with Argo Rollouts, a progressive delivery tool for handling canary and blue/green deployments with robust support for most load balancers in use today. 


## GitOps & Argo CD with Codefresh

Where does Codefresh come into play with GitOps and Argo CD?

Codefresh is the easiest way to get started with GitOps and Argo CD. Codefresh leverages Argo components to have the entire desired state applied from Git to your Kubernetes cluster, and then reported back to Codefresh.

TBD


Every state change operation in Codefresh is made via Git
Codefresh audit log is derived from the Git changelog
Codefresh access control is derived from Git permissions




## GitOps Runtimes 

Codefresh offers two models for GitOps deployments:  

* Hosted GitOps, a fully-managed version of Argo CD. The runtime for Hosted GitOps is hosted on a Codefresh cluster (easy setup) and managed by Codefresh (zero maintenance overhead).  
  If you already have Argo CD installations, this is the option for you. Click once to provision the hosted runtime, and start deploying applications to clusters without having to install and maintain Argo CD.

* Hybrid GitOps has the runtime hosted on the customer cluster and managed by the customer.  
  The hybrid offering retains runtimes within the customer infrastructure while giving you the power of Argo CD with Codefresh’s CI and CD tools, to help achieve continuous integration and continuous delivery goals.

Review [Runtime architecture]({{site.baseurl}}/docs/installation/runtime-architecture/#gitops-architecture).  
For installation, see [Hosted GitOps setup]({{site.baseurl}}/docs/installation/gitops/hosted-runtime/) and [Hybrid GitOps installation]({{site.baseurl}}/docs/installation/gitops/hybrid-gitops/).

## Argo CD applications

Codefresh supports the complete lifecyle to create and deploy Argo CD applications.  
You can create full-featured Argo CD applications in Form or YAML modes. Built-in validations make it easy to identify and fix errors before commit.   

The application is also completely GitOps-compliant. The application manifest is generated, committed to Git, and synced to your cluster.   
See [Creating]({{site.baseurl}}/docs/deployments/gitops/create-application/) and [Managing]({{site.baseurl}}/docs/deployments/gitops/manage-application/) GitOps applications. 


Once deployed, our dashboards give you enterprise-wide visibility into deployments, and key metrics.
From global analytics on applications and deployments (Home dashboard), to failure rate and lead time for changes (DORA dashboard), to application configuration, resource, and rollout management (Applications dashboard). 

See:  
[Home]({{site.baseurl}}/docs/dashboards/home-dashboard/)  
[DORA metrics]({{site.baseurl}}/docs/dashboards/dora-metrics/)  
[Monitoring applications]({{site.baseurl}}/docs/deployments/gitops/applications-dashboard/)  

## Argo Project & Codefresh
Codefresh brings the power of the Argo project to your Kubernetes deployments:

Argo CD for declarative continuous deployment
Argo Rollouts for progressive delivery
Argo Workflows as the workflow engine
Argo Events for event-driven workflow automation framework

Codefresh creates a conformed fork of the Argo project, providing an enterprise-supported version of the same, enhanced with unique functionality.

>Our users rely on the Codefresh platform to deliver software, reliably and predictably without interruption. To maintain that high standard, we add several weeks of testing and bug fixes to new versions of Argo before making them available within Codefresh. Typically, new versions of Argo are available within 30 days of their release.

## Related articles
[Codefresh for CI]({{site.baseurl}}/docs/getting-started/ci-codefresh/) 
[Codefresh for CD]({{site.baseurl}}/docs/getting-started/cd-codefresh/)  
[Concepts in Codefresh]({{site.baseurl}}/docs/getting-started/concepts/)  
