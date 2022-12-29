---
title: "Using Codefresh for CD"
description: "Continuous deployment (CD) with Codefresh pipelines"
group: getting-started
toc: true
---

TBD

Focus on

Connecting K8s cluster
Deploying K8s
Running kubectl
Connecting to Helm registries
Building Helm charts
Pushing Helm charts
Deploying Helm charts
Dashboards


## Connecting to Kubernetes
As continuotus deployment starts with Kubernetes clusters, Codefresh integrates with any known cluster provider suG
Connect your Kubernetes cluster to Codefresh through a simple integration steps. For those Kubernetes clusters that are not inKE the Kubernetes integration window, you will be able to add a cluster from known providers such as Google, Azure, Amazon etc. You can also add any generic Kubernetes cluster by manually entering your cluster settings.

## Deploying to Kubernetes 
Deploying to Kubernetes with Codefresh is easy because of the variety of deployment options you can choose from
So we have Deploy from the Codefresh UI or programmatically through dedicated setos in pipelines
No need for `kubectl` commands thrAs you may have guessed, we have dedicated step types for deployment 
For quick and easy deployment, deploy on-demand from the Codefresh UI
Ddicated steps in pipelines: `deploy` or the more advanced `cf-deploy-kubernetes`step that enables simple templating on Kubernetes manifests.   in a pipeline. Explained in detail in the present page.
Kustomize and Helm as package manager through freestyle steps in pipelines.
Using a freestyle step with Kustomize. Described in details in this page.
Using a freestyle step with your own kubectl commands. This is very flexible, but assumes that you know how to work with kubectl. Described in details in this page.
Using Helm as a package manager. See the Helm quick start guide for more details.

## kubectl
kubectl is the command 
If you wish you can still run your own custom kubectl commands in a freestyle step for maximum flexibility on cluster deployments. Kubectl is the command line interface for managing kubernetes clusters.
Codefresh helps you even in this scenario by automatically setting up your config context with your connected clusters.


## Helm and Codefresh
Codefresh supplies a built-in Helm repository with every Codefresh account. f you also wish to push your chart to a Helm repository (which is always a good practice) you should configure a Helm repository for the step to work with. Besides public HTTP repositories, we support a variety of private, authenticated Helm repositories. Codefresh also provides a free, managed Helm repository for every account.

Connect to Helm repositories
In addition to the official Helm repositories which are displayed in the Helm charts, Codefresh allows you coConnect with any external Helm repository through simple integrations. You can then inject the Helm repository context into your pipelines by selecting the repository name.
Build Helm charts
Install Helm charts or build a new one.
Deploy and puch Helm charts
Codefresh has a Helm step to easily integrate Helm in Codefresh pipelines, and authenticate, configure, and execute Helm commands.
The Helm step can operate in one of 3 modes covering any 

Install to install the chart into a Kubernetes cluster. This is the default mode if not explicitly set.
Push to package the chart and push it to the defined repository.
Authentication to  set up authentication and add the repo to Helm. This is useful if you want to write your own helm commands using the freestyle step’s commands property, but you still want the step to handle authentication.

Deploy Helm charts
Deploy the Helm chart to a Kubernetes cluster or repo or both.  


## Dashboards

