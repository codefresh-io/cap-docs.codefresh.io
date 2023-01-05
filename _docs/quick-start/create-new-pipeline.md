---
title: "Create and run CI pipeline"
description: "Create pipeline to create Docker image"
group: quick-start
toc: true
---

We will start by focusing on the first part of the pipeline overview, the creation of a Docker images.

{% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-ci/docker-build-steps.jpg" 
url="/images/getting-started/quick-start-ci/docker-build-steps.jpg" 
alt="Preparing a Docker image" 
caption="Preparing a Docker image" 
max-width="60%" 
%}

Docker images play a central role in Codefresh pipelines. They are the basic building blocks that serve as the link
between what your source code is producing and what gets deployed. If your own application is not "dockerized" yet, you 
need to create a Dockerfile for it first, before moving it into the Codefresh infrastructure. 

Because all Codefresh capabilities are based on Docker images, Docker is also serving as an abstraction layer over any the implementation language of your source code. Codefresh can work with projects written in Ruby, Python, Java, Node or any other programming language as long as they produce a Docker image. Docker images are a first class citizen in Codefresh pipelines and not just an afterthought.

The example application already comes with its own Dockerfile, making the creation of a Codefresh pipeline very easy.

## Create a project for pipeline



### Before you beigin

* [Connect a Docker registry to Codefresh]({{site.baseurl}}/docs/quick-start/create-docker-registry-integration)

### How to
1. In the Codefresh UI, in the sidebar, from Pipelines, select **Projects**.
1. On the top-right, click **New Project**.
1. Enter the **Project Name**. For example, `my-first-project`.
1. Leave the **Project Tags** empty for the quick start.
1. Select any **Icon** you want. The icon is prefixed to the project name in the Projects page.
1. Click **Create**.  

{% include image.html 
	lightbox="true" 
	file="/images/quick-start/quick-start-ci/create-ci-project.png" 
	url="/images/quick-start/quick-start-ci/create-ci-project.png" 
	alt="Create project for pipeline"
	caption="Create project for pipeline" 
	max-width="70%" 
%}

You now have a new project and can create your first pipeline.

## Create and run the CI pipeline

### How to

1. From the Project page, select the project you created. 
1. Click **Create Pipeline**.
1. Define the following:  
  * **Project**: The project is already selected.
  * **Pipeline Name**: Enter a name for the pipeline.
  * **Add Git repository**: Toggle to on. This setting launches the pipeline when there is a commit to the Git repository.
  * **Add Git clone step to pipeline**: Select the repository with the sample application you forked from the list.
1. Click **Create**.
    In the Workflow tab of the pipeline creation workspace, you'll see that the Inline YAML editor already has a sample YAML.

{% include image.html 
	lightbox="true" 
	file="/images/quick-start/quick-start-ci/create-pipeline.png" 
	url="//images/quick-start/quick-start-ci/create-pipeline.png" 
	alt="Create pipeline settings"
	caption="Create pipeline settings" 
	max-width="70%" 
%}

{:start="5"}
1. Copy and paste the `codefresh.yaml` below into the Inline editor to replace the existing content. 
`codefresh.yml`
{% highlight yaml %}
{% raw %}
version: '1.0'
stages:
  - checkout
  - package
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
    tag: v1.0.0
    dockerfile: Dockerfile
{% endraw %}      
{% endhighlight %}

  The pipeline includes just two steps:
  * A [`git-clone`]({{site.baseurl}}/docs/pipelines/steps/git-clone/) step to check out the code.
    The clone step also uses built-in variables that ensures that the pipeline checks out the exact code described in the commit of the trigger. Don't worry if the exact details are not clear to you yet. 
  * A [`build`]({{site.baseurl}}/docs/pipelines/steps/build/) step to build the docker image **AND** push it to the connected Docker registry.
    The build step uses a [Dockerfile](https://github.com/codefresh-contrib/python-flask-sample-app/blob/master/Dockerfile) that is located at the root folder of the project and creates a Docker image with tag `v1.0.0`.
{:start="6"}
1. To apply your changes, click **Save**. 
  You have completed 
1. To start the pipeline, click **Run**.
1. Retain the default settings, and click **Run** once again.

## View and monitor pipeline build 
When you run the pipeline, Codefresh takes you to the Builds page where you can monitor the build progress of the sample application.

{% include 
image.html 
lightbox="true" 
file="/images/quick-start/quick-start-ci/building.png" 
url="/images/quick-start/quick-start-ci/building.png" 
alt="Monitoring the build run" 
caption="Monitoring the build" 
max-width="50%" 
%}
### Before you begin

### How to
1. Click on the step **Building Docker Image**, and view the logs in the **Output** tab. 
  You can download the logs in HTML or text formats if you want to review them offline. 

The build should complete successfully.

All <!-- is this correct? should it be - future? -->previous runs are displayed in the [Builds page](https://g.codefresh.io/builds).

{% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-ci/finished-build.png" 
url="/images/getting-started/quick-start-ci/finished-build.png" 
alt="Build details" 
caption="Build details (click image to enlarge)" 
max-width="80%" 
%}


## Run unit tests on the Docker image
Unit tests validate the Docker image and confirm that it satisfies the requested functionality, and must be an integral part of the build process.

We will add a new [freestyle step]({{site.baseurl}}/docs/pipelines/steps/freestyle/) to the pipeline's YAML, that runs unit tests.  

Freestyle steps run custom commands within Docker containers. In this case, we run the python command [within the docker image]({{site.baseurl}}/docs/pipelines/variables/#context-related-variables), created from the previous step`MyAppDockerImage` defined by the `image` property)




{% include 
image.html 
lightbox="true" 
file="/images/quick-start/quick-start-ci/unit-test-stage.jpg" 
url="/images/quick-start/quick-start-ci/unit-test-stage.jpg" 
alt="Unit tests workflow" 
caption="Unit tests workflow" 
max-width="80%" 
%}



1. In the Builds page, click the **YAML** tab and then click **Edit Pipeline**.
  The Inline YAML editor in the Workflow tab is displayed. 
1. Paste the following YAML including the new step to run unit tests into the Inline editor:

<!--- Do we need this?>The sample application already has unit tests that can be executed with:
``` 
pip install pytest && pytest
``` -->

`codefresh.yml`
{% highlight yaml %}
{% raw %}
version: '1.0'
stages:
  - checkout
  - package
  - test  
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
    image_name: my-app-image #Change to your image name
    working_directory: ./
    tag: v1.0.1
    dockerfile: Dockerfile
  MyUnitTests:
    title: Running Unit tests
    image: '${{MyAppDockerImage}}'
    stage: test 
    commands: 
      - pip install pytest
      - pytest   
{% endraw %}      
{% endhighlight %}
1. Click **Run** and then click **Run** again to relaunch the build.
1. In the Builds page, you can see a new stage with the step labeled **Running unit tests**.
1. Click the step name to see the test output of the application.

{% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-ci/unit-test-result.png" 
url="/images/getting-started/quick-start-ci/unit-test-result.png" 
alt="Unit tests results" 
caption="Unit tests results (click image to enlarge)" 
max-width="60%" 
%}

> Tip:  
  Codefresh can also run [integration tests]({{site.baseurl}}/docs/pipelines/steps/composition/), and report [test results]({{site.baseurl}}/docs/testing/test-reports/).  
  Therefore, regardless of the type of tests you employ, Codefresh can accommodate your testing process in a fully automated manner as part of the main build.

You have now created a CI pipeline for the sample application.

Codefresh offers extended functionality for pipelines. Read about it in the [YAML documentation]({{site.baseurl}}/docs/pipelines/what-is-the-codefresh-yaml/).

## View Docker images in Codefresh

The Codefresh build logs show that a Docker image is created after each successful build. Where does this image go?

Codefresh has the unique feature where the build step that creates the Docker image, automatically also pushes the image to your default Docker registry! All the images that we have created so far, are stored in the registry you connected at the beginning of the quick start.

{% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-ci/docker-store-stage.jpg" 
url="/images/getting-started/quick-start-ci/docker-store-stage.jpg" 
alt="Automatic storage of Docker images" 
caption="Automatic storage of Docker images" 
max-width="80%" 
%}

1. In the Codefresh UI, expand Artifacts in the sidebar, and click **Images**. 
  A list of Docker images are displayed, sorted by the most recent. This dashboard is populated in real-time with updates from your Docker registry.

{% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-ci/docker-images.png" 
url="/images/getting-started/quick-start-ci/docker-images.png" 
alt="Recent Docker images" 
caption="Recent Docker images (click image to enlarge)" 
max-width="100%" 
%}

<!--
NEED TO BE REVISED ACC TO NEW IMAGES 
Among the information shown, you can clearly see:

* What is the Git Branch that created this image
* What is the Git Hash that contained the last commit

This information can help you to easily correlate the changes that exist in each Docker images, which is very important knowledge when it comes to deployments (explained in detail in the next tutorial).

If you click on a Docker image you will get many more details about it including a timeline of the labels for this Docker image. You also have the ability to enter custom comments that describe any event that you consider important. Codefresh
really shines when it comes to annotating your Docker images with metadata. For more details read the section [Annotations]({{site.baseurl}}/docs/docker-registries/metadata-annotations/).

{% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-ci/docker-timeline.png" 
url="/images/getting-started/quick-start-ci/docker-timeline.png" 
alt="Docker Image timeline" 
caption="Docker Image timeline (click image to enlarge)" 
max-width="50%" 
%}

Codefresh also includes a graphical display of all the layers contained in the Docker image. This can help you identify big layers in your build process and hopefully give you some pointers on how to reduce the size of your deployment binaries.

{% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-ci/docker-layers.png" 
url="/images/getting-started/quick-start-ci/docker-layers.png" 
alt="Docker Layer Analysis" 
caption="Docker Layer Analysis (click image to enlarge)" 
max-width="70%" 
%}

The built-in Docker image storage is very helpful on its own, but it becomes even more impressive when it is coupled with the capability to use it as a basis for temporary demo environments, as we will see in the next section. -->

## (Optional) Upload Docker images to Docker Hub

As we saw, Codefresh automatically pushes the Docker image to the default Docker register. 
You can also upload the same Docker image to a different external registry or make it public in Docker Hub.

The [push pipeline step]({{site.baseurl}}/docs/codefresh-yaml/steps/build/) just pushes an existing Docker image to the registry you define.


{% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-ci/docker-push-stage.jpg" 
url="/images/getting-started/quick-start-ci/docker-push-stage.jpg" 
alt="Pushing a Docker image" 
caption="Pushing a Docker image (click image to enlarge)" 
max-width="80%" 
%}

As the last task in the quick start, we will push our sample application to [Docker Hub](https://cloud.docker.com/),  the free public hosting registry of Docker Inc.
To do so, you will first need to create a free account in Docker Hub and then connect Docker Hub to Codefresh. 
<!--Docker images are one of the central concepts in Codefresh pipelines as everything revolves around them. Powerful Codefresh pipelines can be created by using Docker images as build tools, so it is perfectly normal if you manage a large number of images which are not strictly packaged applications. You may create Docker images that contain building or deployment tools and are used as part of
the build process instead of the build result. -->

1. Create a free account in Docker Hub. 
1. Note down your username and password. 
  In your own projects you can use any other [external registry]({{site.baseurl}}/docs/integrations/docker-registries/) you wish.

  >Docker.io allows you to only push images tagged with your username. If you can do so, create
a Docker Hub account with the same username that you have in Codefresh. If not, you need to change the Docker image
created to match your username.

1. In the Codefresh UI, on the toolbar, click the Settings icon, and then from Configuration, select **Pipeline Integrations**.
1. Select **Docker Registries** and then click **Configure**.
1. From the **Add Registry Provider** dropdown, select **Docker Hub**.
1. Define the following:  
  * **Registry name**: Enter a unique name for this configuration. Note down the regsitry name. You will need it to define it in the pipeline.
  * **Username**: Your Docker Hub username.
  * **Password**: Your Docker Hub password.
  * To verify the connection details, click **Test Connection**. You should see a success message.
1. To apply the changes, click **Save**.
   You have now connected your Docker Hub account to your Codefresh account. 


{% include 
image.html 
lightbox="true" 
file="/images/quick-start/quick-start-ci/add-docker-hub.png" 
url="/images/quick-start/quick-start-ci/add-docker-hub.png" 
alt="Docker Hub registry integration" 
caption="Docker Hub registry integration" 
max-width="60%" 
%}

1. Go back to the Builds page, and click the pipeline to return to the Inline editor. 
1. Paste the following YAML to replace the existing content.
  We now have added a [push step]({{site.baseurl}}/docs/codefresh-yaml/steps/push/) at the end of the pipeline. The image is tagged with the name of the branch.
{% highlight yaml %}
{% raw %}
version: '1.0'
stages:
  - checkout
  - package
  - test 
  - upload    
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
    tag: v1.0.1
    dockerfile: Dockerfile
  MyUnitTests:
    title: Running Unit tests
    image: '${{MyAppDockerImage}}'
    stage: test 
    commands: 
      - pip install pytest
      - pytest      
  MyPushStep:
    title: Pushing to Docker Registry
    type: push
    stage: upload
    tag: '${{CF_BRANCH}}'
    candidate: '${{MyAppDockerImage}}'
    image_name: kkapelon/pythonflasksampleapp #Change kkapelon to your dockerhub username
    registry: dockerhub # Name of your integration as was defined in the Registry screen
{% endraw %}      
{% endhighlight %}
{:start="10"}
1. Replace the `registry` name with the name you specified in the Docker Hub integration settings.
1. To apply your changes, click **Save**, and then click **Run** to start the pipeline again.
1. Click the **Pushing to Docker registry** step to view the log and monitor the progress of the push step.

{% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-ci/docker-pushing.png" 
url="/images/getting-started/quick-start-ci/docker-pushing.png" 
alt="Pushing to Docker Hub" 
caption="Pushing to Docker Hub (click image to enlarge)" 
max-width="70%" 
%}

> Now you have two Docker registries connected to Codefresh. After the build completes execution, the Docker image of the sample application is stored **both** in the default Docker registry and in Docker Hub.

To verify the latter, visit your profile in Dockerhub and look at the image details:

{% include 
image.html 
lightbox="true" 
file="/images/getting-started/quick-start-ci/docker-hub.png" 
url="/images/getting-started/quick-start-ci/docker-hub.png" 
alt="Docker Image details" 
caption="Docker Image details (click image to enlarge)" 
max-width="60%" 
%}


You have now completed the final task in the quick start, and the last step in the build pipeline. Now that we have the basic functionality ready we can see how Codefresh handles [continuous integration](https://en.wikipedia.org/wiki/Continuous_integration) with Pull Requests (PRs) and automatic builds.