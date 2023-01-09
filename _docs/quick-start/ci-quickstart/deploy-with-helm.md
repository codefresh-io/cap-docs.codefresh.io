---
title: "Helm deployment to Kubernetes quick start"
description: "Use the Helm package manager to deploy to a Kubernetes cluster from the Codefresh UI"
group: quick-start
sub_group: ci-quickstart
toc: true
---

The [Kubernetes deployment quick start]({{site.baseurl}}/docs/quick-start/ci-quickstart/deploy-to-kubernetes/) showed you how to quickly deploy an application directly to Kubernetes.  


The Helm quick start will show you how to use [Helm](https://helm.sh/){:target="\_blank"} as a package manager in Codefresh to deploy to Kubernetes. Codefresh comes with an [integrated Helm Repository]({{site.baseurl}}/docs/deplpyments/helm/managed-helm-repository/).

Helm is similar to other package managers (yum, apt, npm, maven, pip, gems), but works at the application level allowing you to deploy multiple manifests together. 




<!--- Helm offers several extra features on top of vanilla Kubernetes deployments, some of which are: 

* The ability to group multiple Kubernetes manifests together and treat them as a single entity (for deployments, rollbacks, and storage). Groups of Manifests are called [Helm Charts](https://helm.sh/docs/topics/charts/).
* Built-in templating for Kubernetes manifests, putting an end to custom template systems that you might use for replacing things such as the Docker tag inside a manifest.
* The ability to create Charts of Charts which contain the templates as well as default values. This means
that you can describe the dependencies of an application in the service level and deploy everything at the same time.
* The ability to create catalogs of applications (Helm repositories) that function similar to traditional package repositories (think npm registry, cpan, maven central, ruby gems etc).  -->


<!---   and has native support for Helm in a number of ways:

1. Easily deploy existing Helm packages to your Kubernetes cluster overriding the default values.
1. Easily create new Helm packages and push them to a Helm repository.
1. View Helm releases and even [perform rollbacks]({{site.baseurl}}/docs/deployments/helm/helm-releases-management/) from the Helm Dashboard.
1. You can [browse Helm packages]({{site.baseurl}}/docs/new-helm/add-helm-repository/)  both from public repositories and your internal Helm repository.
1. You can see Helm releases in the [Environment dashboard]({{site.baseurl}}/docs/deploy-to-kubernetes/environment-dashboard/)
1. You can promote Helm releases with the [Promotion dashboard]({{site.baseurl}}/docs/new-helm/helm-environment-promotion/)  -->


This quick start will:

1. Deploy a Helm application with Codefresh in an automated manner
1. Manage your Helm releases from within Codefresh
1. Store a Helm package inside the integrated Codefresh repository



For reasons of simplicity, we will use the [default Docker registry]({{site.baseurl}}/docs/docker-registries/external-docker-registries/#the-default-registry) that is setup globally in your Codefresh account. For your own application you can also use any other of your registries even if it is not the default.


## Prerequisites
* A [Kubernetes cluster]({{site.baseurl}}/docs/integrations/kubernetes/add-kubernetes-cluster/) in Codefresh
* The Docker registry you connected to your Codefresh account in the CI pipeline quick start 
* An application that has a Dockerfile and a [Helm chart]({{site.baseurl}}/docs/deployments/helm/using-helm-in-codefresh-pipeline/#helm-setup) 
* Cluster with pull access to your default Docker registry
  If not read the [previous guide]({{site.baseurl}}/docs/getting-started/deployment-to-kubernetes-quick-start-guide/#deploying-a-docker-image-to-kubernetes-manually) or look at the [documentation]({{site.baseurl}}/docs/deploy-to-kubernetes/deploy-to-kubernetes/create-image-pull-secret/)

If you want to follow along, feel free to fork this [repository](https://github.com/codefresh-contrib/python-flask-sample-app) in your Git account and look at the [with-helm](https://github.com/codefresh-contrib/python-flask-sample-app/tree/with-helm) branch.

## Deploy a Helm Release to your Kubernetes cluster


1. In the Codefresh UI, expand Ops from the sidebar, and select **Helm Releases**. 
   As you don't have any Helm releases, you see an empty screen.

 {% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-helm/empty-helm-cluster.png" 
url="/images/getting-started/quick-start-helm/empty-helm-cluster.png" 
alt="Empty Helm cluster" 
caption="Empty Helm cluster" 
max-width="70%" 
%}


## Create a pipeline with a Helm step

Codefresh provides a special [Helm step](https://codefresh.io/steps/step/helm){:target="\_blank"} that you can use to perform a deployment.

1. From the Project page, select the project you created. 
1. Click **Create Pipeline**.
1. Define the following:  
  * **Project**: The project is already selected.
  * **Pipeline Name**: Enter a name for the pipeline that will deploy with Helm.
  * **Add Git repository**: Toggle to on. This setting launches the pipeline when there is a commit to the Git repository.
  * **Add Git clone step to pipeline**: Select the repository with the sample application you forked from the list.
1. Click **Create**.
    In the Workflow tab of the pipeline creation workspace, you'll see that the Inline YAML editor already has a sample YAML.
1. Replace the existing content in the Inline YAML editor with the example below.
`YAML`
{% highlight yaml %}
{% raw %}
version: '1.0'
stages:
  - checkout
  - package
  - deploy  
steps:
  clone:
    title: Cloning main repository...
    type: git-clone
    arguments:
      repo: '${{CF_REPO_OWNER}}/${{CF_REPO_NAME}}'
      revision: '${{CF_REVISION}}'
    stage: checkout
  BuildingDockerImage:
    title: Building Docker Image
    type: build
    arguments:
      image_name: my-flask-app
      working_directory: ./python-flask-sample-app
      tag: '${{CF_BRANCH_TAG_NORMALIZED}}-${{CF_SHORT_REVISION}}'
      dockerfile: Dockerfile
    stage: package
  DeployMyChart:
    type: helm
    stage: deploy
    working_directory: ./python-flask-sample-app
    arguments:
      action: install
      chart_name: charts/python
      release_name: my-python-chart
      helm_version: 3.0.2
      kube_context: kostis-demo@FirstKubernetes
      custom_values:
      - 'buildID=${{CF_BUILD_ID}}'
      - 'image_pullPolicy=Always'
      - 'repository=r.cfcr.io/kostis-codefresh/my-flask-app'
      - 'image_tag=${{CF_BRANCH_TAG_NORMALIZED}}-${{CF_SHORT_REVISION}}'
      - 'image_pullSecret=codefresh-generated-r.cfcr.io-cfcr-default'
{% endraw %}
{% endhighlight %}
1. Click **Save** and then **Run** twice to run the pipeline. 

As you can see, the `DeployMyChart` Helm step has three environment variables:
* `chart_name` points to the [chart in the Git repository](https://github.com/codefresh-contrib/python-flask-sample-app/tree/with-helm/charts/python){:target="\_blank"}. 
* `release_name` defines the name of the deployment to be created in the cluster. 
* `kube_context` defines which cluster will be used. The name is the same as that of the cluster you added [Codefresh Integrations](https://g.codefresh.io/account-admin/account-conf/integration/kubernetes).

The step deploys the Helm chart using the default values as found in `values.yaml` inside the chart folder.  
It makes sense to override the defaults using some parameters in the build. For example, instead of tagging the Docker image with the branch name (which is always the same for each build), we could tag it with the hash of the source revision.  

The `custom_values` override the default chart values. The underscores are replaced with dots.
Here we override the name of tag (to match the Docker image built in the previous step) and the pull policy.


You can see the value replacements in the Helm logs inside the pipeline:

 {% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-helm/helm-logs.png" 
url="/images/getting-started/quick-start-helm/helm-logs.png" 
alt="Helm Value replacement" 
caption="Helm Value replacement" 
max-width="100%" 
%}


This is the easiest way to deploy to Kubernetes without having to manually change values in manifests. Helm and Codefresh take care of replacements using the built-in steps.

### View Helm releases 

When a Helm package is deployed to your Kubernetes cluster, Codefresh displays it in the [Helm releases]({{site.baseurl}}/docs/new-helm/helm-releases-management/).

1. In the Codefresh UI, expand Ops from the sidebar, and select **Helm Releases**.
   The new release is displayed.

 {% include 
image.html 
lightbox="true" 
file="/images/quick-start/quick-start-helm/helm-release-details.png" 
url="/images/quick-start/quick-start-helm/helm-release-details.png" 
alt="Helm Releases dashboard with new release" 
caption="Helm Releases dashboard with new release" 
max-width="90%" 
%}

<!--1. If you don't see your release, click the **Settings** icon at the top-right of the cluster, and make sure the correct namespace is selected for your application.

 {% include 
image.html 
lightbox="true" 
file="/images/quick-start/quick-start-helm/helm-version-selection.png" 
url="/images/quick-start/quick-start-helm/helm-version-selection.png" 
alt="Choosing a Helm version" 
caption="Choosing a Helm version (click image to enlarge)" 
max-width="50%" 
%} -->

1. Click on the release and get information regarding its chart, its revisions, changed files and the resulting manifests.

The build values that we defined in the `codefresh.yml` are shown here in a separate tab so it is very easy to 
verify the correct parameters.

 {% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-helm/helm-values.png" 
url="/images/getting-started/quick-start-helm/helm-values.png" 
alt="Helm values" 
caption="Helm values" 
max-width="70%" 
%}

>Tip:  
  You can also go to the [Kubernetes Services dashboard](https://g.codefresh.io/kubernetes/services/){:target="\_blank"} to view the services/pods/deployments that comprise the helm release.


## Roll back a Helm release

With Helm, you can rollback a Helm release to a previous version without actually re-running the pipeline.  
Helm gives you easy rollbacks for free. If you make some commits in your project, Helm retains the same deployment and adds new revisions on it. The server part of Helm keeps a history of all releases, and knows the exact contents of each respective Helm package.

1. In the Codefresh UI, expand Ops from the sidebar, and select **Helm Releases**.
1. Click on the release, and then select the **History** tab.
1. From the list of revisions, select one as the rollback target.


 {% include 
image.html 
lightbox="true" 
file="/images/quick-start/quick-start-helm/helm-rollback.png" 
url="/images/quick-start/quick-start-helm/helm-rollback.png" 
alt="Helm rollback" 
caption="Helm rollback" 
max-width="70%" 
%}


Roll back actually creates a new revision. So, you can go backward and forward in time to any revision.




## Store a Helm chart 

Codefresh includes a [built-in Helm repository]({{site.baseurl}}/docs/new-helm/managed-helm-repository/) available to all accounts. You can store charts in this repository, like any other public Helm repository. You can also manually deploy applications from your repository.

To store a Helm chart, first of all you need to import the shared configuration that defines the integrated Helm Repository, or, you can define the repository URL directly.

Within the pipeline editor click the *Variables* taskbar on the right and click on the vertical ellipsis, select *Add Shared Configuration*. Then, select the shared configuration of the integrated Helm repository and add the variable to your pipeline.

 {% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-helm/import-helm-repo-conf.png" 
url="/images/getting-started/quick-start-helm/import-helm-repo-conf.png" 
alt="Helm settings" 
caption="Import Helm repository configuration (click image to enlarge)" 
max-width="70%" 
%}

Once that is done you add or modify the deploy step in your `codefresh.yml` file:

`YAML`
{% highlight yaml %}
{% raw %}
version: '1.0'
stages:
  - checkout
  - package
  - deploy  
steps:
  clone:
    title: Cloning main repository...
    type: git-clone
    arguments:
      repo: '${{CF_REPO_OWNER}}/${{CF_REPO_NAME}}'
      revision: '${{CF_REVISION}}'
    stage: checkout
  BuildingDockerImage:
    title: Building Docker Image
    type: build
    working_directory: ${{clone}}
    arguments:
      image_name: my-flask-app
      tag: '${{CF_BRANCH_TAG_NORMALIZED}}-${{CF_SHORT_REVISION}}'
      dockerfile: Dockerfile
    stage: package
  deploy:
    title: Storing Helm chart
    type: helm
    stage: deploy
    working_directory: ./python-flask-sample-app
    arguments:
      action: push
      chart_name: charts/python
      helm_version: 3.0.2
      kube_context: 'mydemoAkscluster@BizSpark Plus'
{% endraw %}
{% endhighlight %}

We use the same `helm` step as before. But this time we define `push` as the action (the default is deploying a helm package). In this pipeline we only store the Helm chart in the internal repository.

 {% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-helm/helm-only-store.png" 
url="/images/getting-started/quick-start-helm/helm-only-store.png" 
alt="Storing the chart" 
caption="Storing the chart" 
max-width="100%" 
%}

Once the pipeline finishes, you can see your chart in the *Helm charts* part in the left sidebar.

 {% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-helm/helm-repo.png" 
url="/images/getting-started/quick-start-helm/helm-repo.png" 
alt="Helm settings" 
caption="Import Helm repository configuration (click image to enlarge)" 
max-width="70%" 
%}

There is even an install button if you want to deploy manually the chart. Codefresh will allow to enter your own values
in that case and also select your target cluster.

You can also create a single pipeline that [both stores the chart as well as deploys it in a cluster]({{site.baseurl}}/docs/yaml-examples/examples/helm/). You can learn more about [Helm best practices and Helm pipelines]({{site.baseurl}}/docs/docs/new-helm/helm-best-practices/#helm-concepts) to determine which solution is best.

## What to read next

* [Codefresh built-in Helm repository]({{site.baseurl}}/docs/new-helm/managed-helm-repository/)
* [Using Helm in a Pipeline]({{site.baseurl}}/docs/new-helm/using-helm-in-codefresh-pipeline/)
* [Helm pipeline example]({{site.baseurl}}/docs/yaml-examples/examples/helm/)
* [Working with Docker registries]({{site.baseurl}}/docs/docker-registries/working-with-docker-registries/)
* [Codefresh YAML]({{site.baseurl}}/docs/codefresh-yaml/what-is-the-codefresh-yaml/)


. View Helm releases and even [perform rollbacks]({{site.baseurl}}/docs/deployments/helm/helm-releases-management/) from the Helm Dashboard.
1. You can [browse Helm packages]({{site.baseurl}}/docs/new-helm/add-helm-repository/)  both from public repositories and your internal Helm repository.
1. You can see Helm releases in the [Environment dashboard]({{site.baseurl}}/docs/deploy-to-kubernetes/environment-dashboard/)
1. You can promote Helm releases with the [Promotion dashboard]({{site.baseurl}}/docs/new-helm/helm-environment-promotion/)
For the full list of variables and modes, see [using Helm in Codefresh pipelines]({{site.baseurl}}/docs/deployments/helm/using-helm-in-codefresh-pipeline/).