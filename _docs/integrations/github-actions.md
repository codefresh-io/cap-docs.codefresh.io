---
title: "GitHub Actions"
description: "Connect your GitHub Actions pipelines to Codefresh"
group: integrations
toc: true
---

Codfresh GitOps can be used with any popular Continuous Integration (CI) solution, not just with Codefresh CI.

You can connect any external solution to Codefresh, such as GitHub Actions for example, to take care of common CI tasks such as building/testing/scanning source code, while having Codefresh GitOps still responsible for the deployment. 

### Codefresh Marketplace GitHub Action 

To support the integration between GitHub Actions and Codefresh, we have created a dedicated action in the [Codefresh marketplace](https://github.com/marketplace/actions/csdp-report-image).  

Use the action in the following manner:

1. Create a GitHub Actions pipeline like you normally do.
1. Use any existing CI actions for compiling code, running unit tests, security scanning etc.
1. Place the final action in the pipeline as the "report image" action provided by Codefresh.
1. When the pipeline completes execution, Codefresh retrieves the information on the image that was built and its metadata (essentially the same
data that Codefresh CI would send automatically).
1. View the image in Codefresh in the [Images dashboard]({{site.baseurl}}/docs/pipelines/images/)
 in Codefresh, and is ready to be used with any [GitOps deployment]({{site.baseurl}}/docs/deployment/applications-dashboard/).

 

### Codefresh-GitHub Action integration arguments
The table describes the main arguments required for GitHub Action-Codefresh integration. 


 {: .table .table-bordered .table-hover}
| Argument  | Description     | Required/Optional/Default |
| ---------- |  -------- | ------------------------- |
| `CF_HOST`         | The URL to the cluster with the Codefresh runtime to integrate with. Select from the list of runtimes if you have more than one runtime.  | Required  |
| `CF_API_KEY`         | The API key to interact with Codefresh. Generate the key for the GitHub Action | Required  |
| `CF_CONTAINER_REGISTRY_INTEGRATION`         | The registry integration to use.  | Optional  |
| `CF_JIRA_INTEGRATION`               | The Jira integration to use for the image. Relevant only if Jira enrichment is required for the image. When defined, `CF_ENRICHERS` must include `jira`.  | Optional  |
| `CF_IMAGE`         | The name of the image to report to Codefresh.  | Required  |
| `CF_VERBOSE`         | Enable verbose output. Default is `false`.  | Default  |
| `CF_ENRICHERS`         | The enricher type or types for which to retrieve information and add to the image. When enrichment information is required, can be either `git`, `jira`, or both. By default, empty. When an enricher type is defined, you must also define the required fields for the enrichment-type for Codefresh to retrieve information.  | Default  |
| `CF_WORKFLOW_URL`         | The reported URL of the workflow that builds the image.  | Optional  |
| `CF_WORKFLOW_NAME`         | The name assigned to the workflow that builds the image. When defined, the name is displayed in the Codefresh platform. Example, `CR` | Optional  |
| `CF_GITHUB_TOKEN`         | ???  | Required  |
| `CF_JIRA_PROJECT_PREFIX`         | Relevant only when `CF_ENRICHERS` includes `jira`. The Jira project prefix that identifies the ticket number for which to retrieve information.  | Optional  |
| `CF_JIRA_MESSAGE`         | Relevant only when `CF_ENRICHERS` includes `jira`. The Jira message that should be added to the image  prefix that identifies the ticket number for which to retrieve information.  | Optional  |

| `CF_LOGS_URL`         | The logs view to link this image to.   | Optional  |
| `CF_GIT_REPO`         | Relevant only when `CF_ENRICHERS` includes `git`. The repo containing the source code.  | Optional  |
| `CF_GIT_BRANCH`         | Relevant only when `CF_ENRICHERS` includes `git`. The repo branch containing the source code.  | Optional  |
| `CF_JIRA_MESSAGE`         | Relevant only when `CF_ENRICHERS` includes `jira`. The string that contains PREFIX-ISSUE_ID for Jira.| Optional  |
| `CF_JIRA_FAIL_ON_NOT_FOUND `         | Fail if Jira ticket is not found  | Optional  |


### Configure a GitHub Action in Codefresh


1. In the Codefresh UI, go to [Integrations](https://g.codefresh.io/2.0/account-settings/integrations){:target="\_blank"}.
1. Select **GitHub Actions**, and then click **Configure**.
1. Define the arguments for the GitHub Action. [Review GitHub Action arguments](#codefresh-github-action-integration-arguments). 
  * For the **CF_API_KEY**, click **Generate**. Note down the token is generated.
  * To create a container registry integration for DockerHub if you don't have one, click **Create Container Registry Integration**, and then configure the settings.
  * To create a Jira 
1. Enter this token in GitHub Actions [as a secret](https://docs.github.com/en/actions/security-guides/encrypted-secrets) with the name `CF_API_KEY`.  
  Now you can reference it in all GitHub pipelines as you would any other secret.
1. Leave the other parameters as is. As indicated, Codefresh retrieves the values from the CI tool using the integration name.

{% include image.html 
lightbox="true" 
file="/images/integrations/github-actions/github-action-int-settings.png" 
url="/images/integrations/github-actions/github-action-int-settings.png"
alt="GitHub Action integration for image enrichment"
caption="GitHub Action integration for image enrichment"
max-width="50%"
%}

### GitHub Actions pipeline example

Here is an example pipeline that uses GitHub Actions to build a container image, and the Codefresh action to report the resulting image to Codefresh.  

The example illustrates the differences when you have a Codefresh integration to a CI tool and when you have not:
* GitHub enrichment: The `CF_GITHUB_TOKEN` is required as a credential as there is no integration account.
* Jira enrichment: Because a Jira integration account is configured in Codefresh CI Connect, the step needs only `CF_JIRA_INTEGRATION`, instead of explicit credentials `CF_JIRA_API_TOKEN`, `CF_JIRA_HOST_URL`, and `CF_JIRA_EMAIL`. 


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
          CF_IMAGE: ${{ secrets.DOCKERHUB_USERNAME }}/csdp-report-image-github-action:example-reported-image
        run: |
          docker build . --file Dockerfile --tag $CF_IMAGE && docker push $CF_IMAGE
      - name: report image by action
        with:
          CF_IMAGE: ${{ secrets.DOCKERHUB_USERNAME }}/csdp-report-image-github-action:example-reported-image
          CF_ENRICHERS: "jira, git"
          # Specify cluster app-proxy
          CF_HOST: "https://codefresh.example.com"
          CF_API_KEY: ${{ secrets.CF_API_KEY }}
          CF_CONTAINER_REGISTRY_INTEGRATION: "quay-1"
          
          CF_GITHUB_TOKEN: ${{ secrets.G_TOKEN }} # the GitHub token is required since there is no  integration account
          CF_GIT_PROVIDER: "github"
          CF_GIT_REPO: "codefresh-io/example-github-action-use-csdp-report-image"
          CF_GIT_BRANCH: "feature"

          CF_JIRA_INTEGRATION: codefresh-jira  # the integration name is required, and avoids the need to define the credentials
          CF_JIRA_MESSAGE: "implements PRO-13427"
          CF_JIRA_PROJECT_PREFIX: "PRO"
        uses: codefresh-io/csdp-report-image@0.0.45
{% endraw %}'
{% endhighlight yaml %}

 
## What to read next  
[Adding Git sources]({{site.baseurl}}/docs/runtime/git-sources/)

