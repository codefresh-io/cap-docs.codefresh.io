---
title: "Creating compositions"
description: "Create environment configurations in Codefresh"
group: testing
redirect_from:
  - /docs/on-demand-test-environment/
  - /docs/create-composition/
  - /docs/on-demand-test-environment/create-composition/
  - /docs/share-environment-with-your-test
  - /docs/share-environment-with-your-test/
  - /docs/on-demand-test-environment/share-environment-with-your-test
  - /docs/on-demand-test-environment/share-environment-with-your-test/ 
  - /docs/on-demand-test-environment/share-your-environment-with-your-team/
  - /docs/on-demand-test-environment/test-your-feature/
  - /docs/test-your-feature/     
toc: true
---

Compositions can be launched as part of a unit test step, integration test step, or for running an image for manual testing. You can create compositions from scratch or import an existing `docker-compose.yml` file.

## 1. Add a composition
1. In the Codefresh UI, from the Artifacts section in the sidebar, click [**Compositions**](https://g.codefresh.io/compositions){:target="\_blank"}.
1. Click **Create Composition**.

{% include 
image.html 
lightbox="true" 
file="/images/9b3d250-codefresh_add_composition_first.png" 
url="/images/9b3d250-codefresh_add_composition_first.png"
alt="codefresh_add_composition_first.png" 
max-width="70%"
%}

## 2. Enter the name of composition
In the **Composition Name** text box, type a name for your compositio, and click **Next**.

{% include 
image.html 
lightbox="true" 
file="/images/0c12241-codefresh_name_composition.png" 
url="/images/0c12241-codefresh_name_composition.png"
alt="codefresh_name_composition.png" 
max-width="70%"
%}

## 3. Select starting point for composition

1. Select the type of composition to create:  
  * **From file in repo**: Start a new composition from a Docker Compose file in your repository.
  * **From template**: Use a template as a starting point for the composition, if your repository doesn't include a `docker-compose.yml` file.
  * **Empty composition**: Create a composition from scratch.

{% include 
image.html 
lightbox="true" 
file="/images/ed196d3-codefresh_method_compose.png" 
url="/images/ed196d3-codefresh_method_compose.png"
alt="codefresh_method_compose.png" 
max-width="70%"
%}

{:start="2"}
1. Click **Next**, and continue with one of the following:
  * [From file in repo**](/#from-file-in-repo) 
  * [**From template**](/#from-template)
  * [**Empty composition**](/#empty-composition) (Advanced)


### From file in repo
Start a new composition from a Docker Compose file in your repository.

   {% include 
image.html 
lightbox="true" 
file="/images/10c1ff7-codefresh_compose_add.png" 
url="/images/10c1ff7-codefresh_compose_add.png"
alt="codefresh_compose_add.png" 
max-width="70%"
%}

1. In the search box, type the name of the repository.  
1. If you can't find your repository, click **Add by URL**.  

{% include 
image.html 
lightbox="true" 
file="/images/972337d-codefresh_compose_by_url.png" 
url="/images/972337d-codefresh_compose_by_url.png"
alt="codefresh_compose_by_url.png" 
max-width="70%"
%}

{:start="3"}
1. From the drop-down menu, select a branch for the first build.
1. Click **Next**.
1. Enter the path to `docker-compose.yml`.  
  By default, Codefresh searches for your `docker-compose.yml` at the root level of your repository, by the name _`docker-compose.yml`_. If your `docker-compose.yml` is in a subdirectory, provide the path as well, for example, `./foo/bar/docker-compose.yml`.

{% include 
image.html 
lightbox="true" 
file="/images/dce8b4e-codefresh_path_docker_compose.png" 
url="/images/dce8b4e-codefresh_path_docker_compose.png"
alt="codefresh_path_docker_compose.png" 
max-width="70%"
%}

{:start="6"}
5. Click **Next**.
  >We don’t support the `build` property of Docker Compose. We will replace it with images automatically using a pipeline that is automatically created.

{% include 
image.html 
lightbox="true" 
file="/images/503fa16-codefresh_replace_build.png" 
url="/images/503fa16-codefresh_replace_build.png"
alt="codefresh_replace_build.png" 
max-width="70%"
%}

{:start="7"}
6. Click **Create**.

{% include 
image.html 
lightbox="true" 
file="/images/compositions/composition-list.png" 
url="/images/compositions/composition-list.png"
alt="Composition list" 
max-width="70%"
%}

Your composition is now ready and should appear in the Compositions list.

### From template
If your repository doesn't include a `docker-compose.yml` file, use one of our templates to see how it works!


1. Choose the template.
1. Click **Create**.

{% include 
image.html 
lightbox="true" 
file="/images/bb380f7-codefresh_template_composition.png" 
url="/images/bb380f7-codefresh_template_composition.png"
alt="codefresh_template_composition.png" 
max-width="70%"
%}

  You will see the Composition editor where you can tweak as needed and then launch this composition to see results. 

{% include 
image.html 
lightbox="true" 
file="/images/d8c8e2b-codefresh_template_compose_edit.png" 
url="/images/d8c8e2b-codefresh_template_compose_edit.png"
alt="codefresh_template_compose_edit.png" 
max-width="70%"
%}

{{site.data.callout.callout_info}}
Click on the rocket icon to launch this composition.
{{site.data.callout.end}}

### Empty composition
Create a composition from scratch. You will see the following screen with an empty composition. 


1. To add a service, click the **Add Service** button.  
  You can add existing built services, or provide the name for the Docker image that will be pulled from the Docker registry.
1. (Optional) Click **Edit**, and modify the content based on [Docker Compose YAML ](https://docs.docker.com/compose/compose-file/){:target="_blank"}.

{% include 
image.html 
lightbox="true" 
file="/images/327ce99-codefresh_empty_compose.png" 
url="/images/327ce99-codefresh_empty_compose.png"
alt="codefresh_empty_compose.png" 
max-width="70%"
%}

{:start="3"}
1. Click **Save** on the upper-right corner.

## Manually launching compositions

When you are ready with the composition, launch it to inspect your application. Launching a composition, creates a temporary
test environment in your Codefresh account that you can use to inspect your application.

1. In the Codefresh UI, from the Artifacts section in the sidebar, click [**Compositions**](https://g.codefresh.io/compositions){:target="\_blank"}.
1. Select the composition to launch.

{% include 
image.html 
lightbox="true" 
file="/images/compositions/composition-list.png" 
url="/images/compositions/composition-list.png"
alt="Composition list" 
max-width="70%"
%}

{:start="3"}
1. Click the **Launch** icon.

{% include 
image.html 
lightbox="true" 
file="/images/e620ba6-compose-launch-button.png" 
url="/images/e620ba6-compose-launch-button.png"
alt="compose-launch-button.png" 
max-width="70%"
caption="click on image to enlarge"
%}

{:start="4"}
1. To verify that the launch completed successfully, review the log.

{% include 
image.html 
lightbox="true" 
file="/images/b84878c-composition-launch-log.png" 
url="/images/b84878c-composition-launch-log.png"
alt="composition-launch-log.png" 
max-width="70%"
%}

## Working with existing compositions

You can edit any composition you created, regardless of the method used to create it, by clicking on its name from the compositions list.
Edit the details of the composition in the YAML editor.


{% include 
image.html 
lightbox="true" 
file="/images/0aa5fba-codefresh_first_composition.png" 
url="/images/0aa5fba-codefresh_first_composition.png"
alt="codefresh_first_composition.png" 
max-width="70%"
%}



## Sharing the environment URL

After you successfully spin up a composition, click the **Running compositions** tab from the *Compositions* item in the left pane, to view a record for the running environment and all containers for the environment.

{% include 
image.html 
lightbox="true" 
file="/images/compositions/environment-running.png" 
url="/images/compositions/environment-running.png"
alt="environment-running.png" 
max-width="70%"
caption="Active test environments"
alt="Active test environments"
%}

Click the **Hashtag** icon to share your environment with your team.

{% include 
image.html 
lightbox="true" 
file="/images/1fbb802-Screen_Shot_2017-01-31_at_2.58.58_PM.png" 
url="/images/1fbb802-Screen_Shot_2017-01-31_at_2.58.58_PM.png"
alt="Sharing environment link" 
caption="Sharing environment link" 
max-width="70%"
%}

## Related articles
[Unit tests]({{site.baseurl}}/docs/testing/unit-tests/)   
[Integration tests]({{site.baseurl}}/docs/testing/integration-tests/)    
[Creating test reports]({{site.baseurl}}/docs/testing/test-reports/)  
