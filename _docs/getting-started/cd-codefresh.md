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
Continuous deployment starts with Kubernetes clusters, and Codefresh integrates with any known cluster provider for Kubernetes through a few simple steps. Connect your Google, Azure, Amazon Kubernetes cluster to Codefresh through simple integration steps. 
For those Kubernetes clusters that are not in our list of cluster providers, you can manually enter your cluster settings to add any generic Kubernetes cluster.

## Deploying to Kubernetes 
Codefresh offers a variety of options for you to choose from when deploying to Kubernetes.
We have deployment from the Codefresh UI or programmatically through dedicated steps in pipelines, avoiding the need for `kubectl` commands thrAs you may have guessed, we have dedicated step types for deployment 

On-demand deployment:  
For quick and easy deployment, deploy on-demand from the Codefresh UI
Dedicated steps in pipelines: Use our `deploy` step, or the more advanced `cf-deploy-kubernetes`step that enables simple templating on Kubernetes manifests.   
Kustomize through a freestyle step
Helm as package manager, also through a freestyle step  

Finally, if you are familair with and want to work with 'kubectl', use your own our freestyle step If you wish you can still run your own custom kubectl commands in a freestyle step for maximum flexibility on cluster deployments. 

## kubectl
kubectl is the command line interface for managing kubernetes clusters.
Codefresh automatically sets up your config context with your connected clusters.


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

