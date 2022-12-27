---
title: "Using Codefresh for CI/CD"
description: "Continuous integration (CI) and continuous deployment (CD) with Codefresh pipelines"
group: getting-started
toc: true
---

TBD

What's different in Codefresh when it comes to continuous integration (CI) and continuous delivery (CD)?
There is a long answer and a short one.

Let's start with the short answer, that illustrates how Codefresh differs from and how it is superior to other CI/CD solutions. 

Codefresh:
* Is a complete CI/CD solution, and not just CI.
* Works with all major Git platforms and cloud providers. There is no lock-in with any particular vendor.
* Has several unique features such as a distributed Docker layer cache, an auto-mounted shared volume, a private Docker registry and a private Helm repository.
* Built-in Kubernetes dashboard
* Built-in Helm dashboard, Helm charts browser, and Helm environment board 

Distilled, Codefresh covers the full software lifecycle. You can see a release in the Kubernetes dashboard, click on it and go to the Docker image, click on the Docker image and go to the build that created it, all from a single interface. Codefresh is turbo-charged CI/CD.
 Q. How is Codefresh different than Jenkins?
A. Codefresh is a superset of Jenkins. Jenkins is only CI. You need to write custom scripts or use another tool such as Ansible to deploy with Jenkins. See the comparison matrix and the detailed blog post. There is also a comparison with Jenkins X.

Q. How is Codefresh different than my custom deployment scripts in bash/Ansible/Chef/Puppet/Python?
A. These scripts are custom made, complex to maintain and difficult to read. One of the reasons that developers and operators have difficulties in communication is the in-house nature of deployment scripts. Codefresh allows you to create standard declarative pipelines where each step is a reusable Docker image.

Everything for CI/CD in Codefresh starts and ends with pipelines. In Codefresh, a pipeline can do pretty much anyhting, only CI, only CD, both CI and CD.

Your CI pipeline can compile and package code, build and push Docker images. The CD pipeline can deploy applications/artifacts to VMs, Kubernetes clusters, FTP sites, S3 buckets, and more. And yet another pipeline combines both integration and deployment for full CI/CD.
Other pipelines can run unit tests, integration tests, acceptance tests etc. CI/CD pipeline to create and deploy your applications, or a pipeline run any custom action, such as tests.

Completely programmatic approach
Unlike other CI/CD platforms which can be tightly coupled to a single Git provider, or a specific vendor or set of tools, Codefresh supports a fully programmtic implementation. create pipelines and define the pipeline’s steps, triggers, and variables.

Everything in the pipeline is defined as code and applied with the command line. Storing the definitions in a code repo ensures consistenct. Upscaling or expanind Doing so means we can create all of our pipelines in a consistent way and store those definitions in a code repository. Taking this one step step deeper, we could then create a bootstrap pipeline in Codefresh that generates pipelines when new definitions are added to this repo. See our previous post Programmatic Creation of Codefresh Pipelines (part 2) for more on this.


The Docker registry integrations and all cluster integrations are automatically available to all pipelines. You don’t need docker login commands or kubectl commands to set up a Kube context inside your pipeline.



Pipelines for 
Everything in the CI/CD  in Codefresh starts and ends with pipelines. In Codefresh, a ppieplene can do pretty much anyhting.

It can be a set of definitions on how to implement the CI/CD for your applications, or it can run any custom action, such as unit, acceptance tests.
A CI pipeline would then fcompile and package code, build Docker images and push them Docjer  images,. A CD pipeline woulddeploying applications/artifacts to VMs, Kubernetes clusters, FTP sites, S3 buckets and more. And a CI/CD pipeline does all of the above.
Pipelines can Run unit tests, integration tests, acceptance tests etc.





Pipeline extras
We support all Git providers: Both on-premises and cloud 
provides a rich set of triggers: Trigger pipelines  on a schedule, from a push to a docker registry or from a push to a helm registry or from actions that happened in Git.
Has a rich, modern API With Codefresh, you can trigger a pipeline

Since Codefresh is decoupled from any single pipeline source, we provide a programmatic way to 


## CI/CD pipelines

As mentioned earlier, everything in Codefresh CI/CD starts and ends with pipelines. 
A Codefresh pipeline has two distinct 

* Pipeline specifications in YAML
  The specifications define the pipeline
  * Metadata such as name, project, tags, in Codefresh
  * Events that trigger the pipeline such as webhooks, cron events, etc.)
  * The steps to use for this pipeline (inline, from a repo, etc.)

* Pipeline steps, that define:
  * Jobs to run
  * Sequence in which to run the jobs
  * CI and our CD processes

Docker registry integrations and all cluster integrations are automatically available to all pipelines. You don’t need docker login commands or kubectl commands to setup a Kube context inside your pipeline.
Codefresh covers the full software lifecycle. You can see a release on the cluster dashboard, click on it, go to the docker image, click on it and go the build that created it, all from a single interface. Codefresh is batteries-included CI/CD.

For more on pipelines, start with the Introduction to Codefresh pipelines or jump to pipeline creation with ????

## Releases and dashboards

## Where to go from here

Quick starts
To get up and running, follow our quick starts. The quick start modules are a series of flows that guide you from setting up your first account in Codefresh, to creating a basic pipeline, and deploying to Kubernetes.
See XREF TBD 

Example catalog
For those who are familiar with CI/CD, we have an extensive collection of examples, covering several CI and CD scenarios:
CI examples
CD examples

Guides
And finally, if you want more meat, dive in to our detailed guides.
XREF TBD

Codefresh has extensive support for deployments to Kubernetes, and for more taditional deployment environments such as to plain VMs, JAR files, static websites, Wordpress instances, Database changesets .
B
 UI support, 
Dedicated pipeline steps: 
  deploy 
  cf-deploy-kubernetes
Kustomize
kubectl
Helm
Using the Codefresh GUI to deploy on demand. This is the easiest way and was described in the quick start guide.
Using the dedicated deploy step in a pipeline. Explained in detail in the present page.
Using the cf-deploy-kubernetes step in a pipeline. This can also perform simple templating on Kubernetes manifests.
Using a freestyle step with Kustomize. Described in details in this page.
Using a freestyle step with your own kubectl commands. This is very flexible, but assumes that you know how to work with kubectl. Described in details in this page.
Using Helm as a package manager. See the Helm quick start guide for more details.


