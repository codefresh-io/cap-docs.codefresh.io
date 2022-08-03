---
title: "GitHub Actions"
description: "Connect your GitHub Actions pipelines to Codefresh"
group: integrations
toc: true
---

Codefresh Hosted GitOps can be used with any popular Continuous Integration (CI) solution, not just with Codefresh CI.

You can connect any external CI solution to Codefresh, such as GitHub Actions for example, to take care of common CI tasks such as building/testing/scanning source code, with Codefresh Hosted GitOps still responsible for the deployment, including image enrichment and reporting.  
See [Image enrichment with integrations]({{site.baseurl}}/docs/integrations/image-enrichment-overview/).


### Codefresh Marketplace GitHub Action 

To support the integration between GitHub Actions and Codefresh, we have created a dedicated action in the [Codefresh marketplace](https://github.com/marketplace/actions/codefresh-report-image){:target="\_blank"}. The action combines image enrichment and reporting through integrations with tracking tools, such as Jira, and container registries, such as DockerHub or Quay.

Use the action in the following manner:

1. Create a GitHub Actions pipeline like you normally do.
1. Use any existing CI actions for compiling code, running unit tests, security scanning etc.
1. Place the final action in the pipeline as the "report image" action provided by Codefresh, copying the arguments and values from the GitHub Actions integration you set up in Codefresh.
1. When the pipeline completes execution, Codefresh retrieves the information on the image that was built and its metadata (essentially the same data that Codefresh CI would send automatically).
1. View the image in Codefresh in the [Images dashboard]({{site.baseurl}}/docs/deployment/images/), and in any [GitOps deployment]({{site.baseurl}}/docs/deployment/applications-dashboard/) in which it is used.

 

### Codefresh-GitHub Action integration arguments
The table describes the arguments required for GitHub Action-Codefresh integration. 


 {: .table .table-bordered .table-hover}
| Argument  | Description     | Required/Optional/Default |
| ---------- |  -------- | ------------------------- |
| `CF_HOST`                      | Deprecated from v 0.0.456 and higher. This is because the URL as the URL can change and can fail the enrichment. Recommend using `CF_RUNTIME_NAME` instead. The URL to the cluster with the Codefresh runtime to integrate with. If you have more than one runtime, select the runtime from the list. Codefresh displays the URL of the selected runtime cluster.  | Required  |
| `CF_RUNTIME_NAME`              | The name of the runtime to use for this integration. is this a dropdown??  | Required  |
| `CF_API_KEY`                   | The API key to authenticate the GitHub Actions user to Codefresh. Generate the key for the GitHub Action. | Required  |
| `CF_CONTAINER_REGISTRY_INTEGRATION` | The name of the registry integration created in Codefresh to use with the GitHub Action.  | Optional  |
| `CF_JIRA_INTEGRATION`               | The name of the Jira integration created in Codefresh to use for the GitHub Action. Relevant only if Jira enrichment is required for the image. If you don't have a Jira integration, create a new integration (see [Jira integration]({{site.baseurl}}/docs/integrations/jira/)).   | Optional  |
| `CF_IMAGE`                    | The path to the image to enrich and report to Codefresh.  | Required  |
| `CF_WORKFLOW_NAME`           | The name assigned to the workflow that builds the image. When defined, the name is displayed in the Codefresh platform. Example, `Staging step` | Optional  |
| `CF_GIT_BRANCH`              | The Git branch with the commit.  | Required  |
| `CF_GITHUB_TOKEN`            | The GitHub PAT (Personal Access Token) to use for Git integration. The token must have `repo` scope. See [Git tokens]({{site.baseurl}}/docs/reference/git-tokens/). | Required  |
| `CF_JIRA_PROJECT_PREFIX`     | Relevant only when `CF_JIRA_INTEGRATION` is defined. The Jira project prefix that identifies the ticket number for which to retrieve information.  | Required  |
| `CF_JIRA_MESSAGE`            | Relevant only when `CF_JIRA_INTEGRATION` is defined. The Jira issue (the issue ID and the string describing the issue) for the `CF_JIRA_PROJECT_PREFIX` to associate with the image.  | Required  |
| `CF_JIRA_FAIL_ON_NOT_FOUND`            | Relevant only when `CF_JIRA_INTEGRATION` is defined. The report image action when the `CF_JIRA_MESSAGE` is not found. When set to `true`, the report image action is failed.  | Required  |



### Connect a GitHub Action in Codefresh

Add a GitHub Action to Codefresh with the required arguments. 
1. In the Codefresh UI, go to [Integrations](https://g.codefresh.io/2.0/account-settings/integrations){:target="\_blank"}.
1. Select **GitHub Actions**, and then click **Configure**.
1. Define the arguments for the GitHub Action. [Review GitHub Action arguments](#codefresh-github-action-integration-arguments). 
  * For the **CF_API_KEY**, click **Generate**. Note down the token generated.
  * To create a container registry integration if you don't have one, click **Create Container Registry Integration**, and then select the one to add. See the Related articles for links to container registry documentation.
  * To create a Jira integration, click **Create Atlassian Jira Integration**, and then configure the settings. See [Jira integration]({{site.baseurl}}/docs/integrations/jira/).
1. Enter this token in GitHub Actions [as a secret](https://docs.github.com/en/actions/security-guides/encrypted-secrets) with the name `CF_API_KEY`.  
  Now you can reference it in all GitHub pipelines as you would any other secret.

{% include image.html 
lightbox="true" 
file="/images/integrations/github-actions/github-action-int-settings.png" 
url="/images/integrations/github-actions/github-action-int-settings.png"
alt="GitHub Action integration for image enrichment"
caption="GitHub Action integration for image enrichment"
max-width="50%"
%}

{:start="5"}
1. To generate the manifest for the report image action, click **Generate Manifest** above the argument list.
1. In the generated manifest, add the required fields and values.
1. Click **Copy** and then paste the copied snippet into the GitHub Actions pipeline. See the example below for guidelines.

### GitHub Actions pipeline example

Here is an example pipeline that uses GitHub Actions to build a container image, and the Codefresh action to enrich and report the resulting image to Codefresh.  

Because a Jira integration account is configured in Codefresh, the step needs only the name for `CF_JIRA_INTEGRATION`, instead of explicit credentials `CF_JIRA_API_TOKEN`, `CF_JIRA_HOST_URL`, and `CF_JIRA_EMAIL`. 


{% highlight yaml %}
{% raw %}

name: Docker Image CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
jobs:
  build:
    environment:
      name: test
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Login to DockerHub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      - name: Build & push the Docker image
        env:
          CF_IMAGE: ${{ secrets.DOCKERHUB_USERNAME }}/build-by-github-action:0.0.1
        run: |
          docker build . --file Dockerfile --tag $CF_IMAGE && docker push $CF_IMAGE
          echo "Image should be accessible to your local machine (after docker login) by:"
          echo "docker pull $CF_IMAGE"
          docker pull $CF_IMAGE
          echo "On the next step, the report image would use the integration to pull information on the reported image, and using the specified enrichers."
      - name: report image by action
        with:
          # Name of runtime to implement the enrichment
          CF_RUNTIME_NAME: '[runtime_name]'

          # Codefresh API key !! Committing a plain text token is a security risk. We highly recommend using an encrypted secrets. !!
          # Documentation - https://docs.github.com/en/actions/security-guides/encrypted-secrets
          CF_API_KEY: ${{ secrets.USER_TOKEN }}

          # Name of Container registry integration
          CF_CONTAINER_REGISTRY_INTEGRATION: "docker"

          # The git branch which is related for the commit
          CF_GIT_BRANCH: 'main'

          # Image path to enrich 
          CF_IMAGE: ${{ secrets.DOCKERHUB_USERNAME }}/build-by-github-action:0.0.1

          # GitHub Access token !! Committing a plain text token is a security risk. We highly recommend using an encrypted secrets. !!
          # Documentation - https://docs.github.com/en/actions/security-guides/encrypted-secrets
          CF_GITHUB_TOKEN: ${{ secrets.CF_GITHUB_TOKEN }}    

          # String starting with the issue ID to associate with image
          CF_JIRA_INTEGRATION: "jira" 

          CF_JIRA_MESSAGE: "A message with embedded issue ( i.e. CR-11027 ) that would be use query jira for the ticket "

          # Jira project filter
          CF_JIRA_PROJECT_PREFIX: "CR"
        uses: codefresh-io/codefresh-report-image@latest
        
{% endraw %}'
{% endhighlight yaml %}

### GitHub Action logs
View and analyze logs for GitHub Action workflows through the Logs tab. When a GitHub Action is run, it is added to the Logs tab.  
You can:  
* Filter by status or by date range to view a subset of actions
* Navigate to the build file in GitHub Actions, and view the Codefresh report image step

{% include image.html 
lightbox="true" 
file="/images/integrations/github-actions/github-actions-logs.png" 
url="/images/integrations/github-actions/github-actions-logs.png"
alt="GitHub Action: Logs tab"
caption="GitHub Action: Logs tab"
max-width="50%"
%}

**Build YAML in GitHub Action**  

The Run column includes the link to the build files for the actions.  

Here are examples of the build file for the GitHub Action (top) and of the Codefresh report image step in the action (below).

{% include image.html 
lightbox="true" 
file="/images/integrations/github-actions/action-build-yaml.png" 
url="/images/integrations/github-actions/action-build-yaml.png"
alt="Build file in GitHub Action"
caption="Build file in GitHub Action"
max-width="50%"
%}

{% include image.html 
lightbox="true" 
file="/images/integrations/github-actions/actiosn-cf-report-image-step.png" 
url="/images/integrations/github-actions/actiosn-cf-report-image-step.png"
alt="Codefresh report image step in GitHub Action build file"
caption="Codefresh report image step in GitHub Action build file"
max-width="50%"
%}


### Related articles
[Docker Hub integration]({{site.baseurl}}/docs/runtime/dockerhub/)
[Quay integration]({{site.baseurl}}/docs/runtime/quay/)  
[Add Git sources to runtimes]({{site.baseurl}}/docs/runtime/git-sources/)  
[Shared configuration repo]({{site.baseurl}}/docs/reference/shared-configuration)


