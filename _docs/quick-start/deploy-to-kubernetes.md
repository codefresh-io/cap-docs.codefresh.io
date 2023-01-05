---
title: "Kubernetes deployment quick start"
description: "How to deploy to a Kubernetes cluster from the Codefresh UI"
group: quick-start
sub_group: ci-quickstart
toc: true
---

In this quick start, you'll learn how to deploy the Docker image you created to a Kubernetes cluster, both manually through the Codefresh UI, and automatically through a pipeline to redeploy the image when there are changes in the source code.

WeNotice that for this tutorial we will use the GUI provided by Codefresh to both create the Kubernetes service inside the cluster and also to create the CI/CD pipeline that keeps it up to date. In a real world scenario it is best if you use  [Codefresh YAML]({{site.baseurl}}/docs/codefresh-yaml/what-is-the-codefresh-yaml/) which is much more powerful and flexible.

Codefresh also offers [several alternative ways]({{site.baseurl}}/docs/deploy-to-kubernetes/deployment-options-to-kubernetes/) of deploying to Kubernetes.


At the end of this tutorial we will have a pipeline that: 

1. Checks out code from GitHub and creates a Docker image
1. Stores it in the default Docker registry connected to your Codefresh account
1. Notifies the Kubernetes cluster that a new version of the application is present. Kubernetes will pull the new image and deploy it

 {% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-k8s/overview.png" 
url="/images/getting-started/quick-start-k8s/overview.png" 
alt="Deployment overview" 
caption="A complete CI/CD pipeline" 
max-width="80%" 
%}



## Prerequisites

 [Kubernetes cluster]({{site.baseurl}}/docs/deploy-to-kubernetes/add-kubernetes-cluster/) in Codefresh
  * [Docker registry]({{site.baseurl}}/docs/s/external-docker-registries/) in your Codefresh account
  * Either our sample application or your own application that has a Dockerfile. 

>For the quick start, you **don't** need a Kubernetes deployment file. Codefresh creates one for you via the UI. 



## Manually deploy Docker image to Kubernetes

Deploy the Docker image to your Kubernetes cluster without writing any configuration files at all.

1. Get the name of the Docker image you created:
    1. In the Codefresh UI, expand Artifacts in the sidebar, and select **Images**.
    1. Go to the **
Go to the Build

 {% include 
image.html 
lightbox="true" 
file="/images/quick-start/quick-start-k8s/add-service.png" 
url="/images/quick-start/quick-start-k8s/add-service.png" 
alt="Add Kubernetes service" 
caption="Add Kubernetes service" 
max-width="70%" 
%}




1. In the Codefresh UI, expand Ops from the sidebar, and select **Kubernetes Services**.
  Codefresh displays the deployments (pods and namespaces) in your Kubernetes cluster.
1. On the top-right, click **New**, and then select **Add Service**.

{% include 
image.html 
lightbox="true" 
file="/images/quick-start/quick-start-k8s/add-service-button.png" 
url="/images/quick-start/quick-start-k8s/add-service-button.png" 
alt="Codefresh Kubernetes Dashboard" 
caption="Codefresh Kubernetes Dashboard" 
max-width="70%" 
%}

{:start="3"}
1. Create a Kubernetes deployment (and associated service):
  * **Cluster**: The cluster to which to deploy your image. If you have more than one cluster, select the cluster.
  * **Namespace**: The namespace in the cluster to which to deploy the application. For the quick start, retain **default**.
  * **Service Name**: An arbitrary name for your service.
  * **Replicas**: The number of replicas to create for resiliency. For the quick start, we'll define **1**.
  * **Expose Port**: Select to make your application available outside the cluster and users can access it.
  * **Image**: The fully qualified name of your Docker image. Get the image name from the Builds page (select pipeline, and select the  Images tab)
  * **Image Pull Secret**: Select your default Docker registry and create a pull secret for it. <!--- not clear -->
  * **Internal Ports**: The port exposed from your application. The example Python app we deploy, exposes `5000`.
 




You can see the full name of the Docker image in [Images](https://g.codefresh.io/images/) > Summary tab.

 {% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-k8s/docker-image-name.png" 
url="/images/getting-started/quick-start-k8s/docker-image-name.png" 
alt="Finding the full name of a Docker image" 
caption="Finding the full name of a Docker image (click image to enlarge)" 
max-width="60%" 
%}

By default, Codefresh appends the branch name of a git commit to the resulting Docker image. This is why
in the *Image* field we used the branch name as tag

>Do not use `latest` for your deployments. This doesn't help you to understand which version is deployed. Use
either branch names or even better git hashes so that you know exactly what is deployed on your Kubernetes cluster. Notice also that the YML manifest
Codefresh is creating has an image pull policy of `always`, so the cluster will always redeploy the latest image even if it has the same name as the previous one.

Finally click the *deploy* button. Codefresh will create a Kubernetes YAML file behind the scenes and apply it to your
Kubernetes cluster. The cluster will contact the Codefresh registry and pull the image. The cluster will then create all the needed resources (service, deployments, pods) in order to make the application available.

You can watch the status of the deployment right from the Codefresh UI.

 {% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-k8s/after-deployment.png" 
url="/images/getting-started/quick-start-k8s/after-deployment.png" 
alt="Codefresh K8s deployment" 
caption="Codefresh K8s deployment (click image to enlarge)" 
max-width="70%" 
%}

Once the deployment is complete, you will also see the public URL of the application. You can visit it in the browser
and see the application running.

 {% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-k8s/before-change.png" 
url="/images/getting-started/quick-start-k8s/before-change.png" 
alt="Example Python Application" 
caption="Example Python Application (click image to enlarge)" 
max-width="50%" 
%}

This concludes the manual deployment. We deployed a Docker image from Codefresh to a Kubernetes cluster without
writing any YAML files at all! The next step is to automate this process so that every time a commit happens in git, the application will be redeployed.

## Automating deployments to Kubernetes

The application is now running successfully in the Kubernetes cluster. We will setup a pipeline in Codefresh
so that any commits that happen in GitHub, are automatically redeploying the application, giving us a true CI/CD pipeline.

To do this, we will add two extra steps in the basic pipeline created in the [previous tutorial]({{site.baseurl}}/docs/getting-started/create-a-basic-pipeline/).

Here is the complete pipeline:

`codefresh.yml`
{% highlight yaml %}
{% raw %}
version: '1.0'
stages:
  - checkout
  - package
  - test 
  - upload
  - deploy
steps:
  main_clone:
    title: Cloning main repository...
    type: git-clone
    repo: '${{CF_REPO_OWNER}}/${{CF_REPO_NAME}}'
    revision: '${{CF_REVISION}}'
    stage: checkout
  MyAppDockerImage:
    title: Building Docker Image
    type: build
    stage: package
    image_name: my-app-image
    working_directory: ./
    tag: '${{CF_BRANCH}}'
    dockerfile: Dockerfile
    disable_push: true
  MyUnitTests:
    title: Running Unit tests
    image: '${{MyAppDockerImage}}'
    stage: test 
    commands:
      - python setup.py test    
  MyPushStep:
    title: Pushing to DockerHub Registry
    type: push
    stage: upload
    tag: '${{CF_BRANCH}}'
    candidate: '${{MyAppDockerImage}}'
    image_name: kkapelon/pythonflasksampleapp #Change kkapelon to your dockerhub username
    registry: dockerhub # Name of your integration as was defined in the Registry screen
  DeployToMyCluster:
    title: deploying to cluster
    type: deploy
    stage: deploy
    kind: kubernetes  
    ## cluster name as the shown in account's integration page
    cluster:  my-demo-k8s-cluster
    # desired namespace
    namespace: default
    service: python-demo
    candidate:
      # The image that will replace the original deployment image 
      # The image that been build using Build step
      image: kkapelon/pythonflasksampleapp:${{CF_BRANCH}}
      # The registry that the user's Kubernetes cluster can pull the image from
      # Codefresh will generate (if not found) secret and add it to the deployment so the Kubernetes master can pull it
      registry: dockerhub   
{% endraw %}      
{% endhighlight %}

You can see that we have added a new [deploy step]({{site.baseurl}}/docs/codefresh-yaml/steps/deploy/) at the end of the pipeline. Deploy steps allow you to deploy Kubernetes applications in a declarative manner. Codefresh offers many more [ways for Kubernetes deployments]({{site.baseurl}}/docs/deploy-to-kubernetes/deployment-options-to-kubernetes/).

The deploy step will update an *existing* Kubernetes deployment and will optionally create a [pull secret]({{site.baseurl}}/docs/deploy-to-kubernetes/access-docker-registry-from-kubernetes/) for the image if needed, but it will not create any Kubernetes services (which is ok in our case as we created it manually in the previous section).

Once all the details are filled in the pipeline editor, click the *Save* button.

Now we will change the application in the production branch and commit/push the change to Git.

 {% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-k8s/git-change.png" 
url="/images/getting-started/quick-start-k8s/git-change.png" 
alt="Git change" 
caption="Git change (click image to enlarge)" 
max-width="70%" 
%}

Codefresh will pick the change automatically and [trigger]({{site.baseurl}}/docs/configure-ci-cd-pipeline/triggers/) a new build that deploys the new version:



 {% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-k8s/deployment-build.png" 
url="/images/getting-started/quick-start-k8s/deployment-build.png" 
alt="Codefresh K8s deployment" 
caption="Codefresh K8s deployment (click image to enlarge)" 
max-width="90%" 
%}


Once the build is complete, if you visit again the URL you will see your change
applied.

 {% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-k8s/after-change.png" 
url="/images/getting-started/quick-start-k8s/after-change.png" 
alt="Example Python Application after change" 
caption="Example Python Application after change (click image to enlarge)" 
max-width="50%" 
%}

You now have a complete CI/CD pipeline in Codefresh for fully automated builds to Kubernetes!

## What to read next

* [Deploying to Kubernetes with Helm]({{site.baseurl}}/docs/getting-started/helm-quick-start-guide/)
* [Kubernetes deployment methods]({{site.baseurl}}/docs/deploy-to-kubernetes/deployment-options-to-kubernetes/)
* [Introduction to Pipelines]({{site.baseurl}}/docs/configure-ci-cd-pipeline/introduction-to-codefresh-pipelines/)
* [Working with Docker registries]({{site.baseurl}}/docs/docker-registries/working-with-docker-registries/)
* [Codefresh YAML]({{site.baseurl}}/docs/codefresh-yaml/what-is-the-codefresh-yaml/)












