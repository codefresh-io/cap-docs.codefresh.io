---
title: "GitHub Actions"
description: "Connect your GitHub Actions pipelines to Codefresh"
group: integrations
toc: true
---

Codfresh GitOps can be used with any popular Continuous Integration (CI) solution, not just with Codefresh CI.

You can connect any external solution to Codefresh, such as GitHub Actions for example, to take care of common CI tasks such as building/testing/scanning source code, with Codefresh Hosted GitOps still responsible for the deployment, including image enrichment and reporting.

### Codefresh Marketplace GitHub Action 

To support the integration between GitHub Actions and Codefresh, we have created a dedicated action in the [Codefresh marketplace](https://github.com/marketplace/actions/csdp-report-image). The action combines image enrichment and reporting through integrations with Jira, and container registries such as DockerHub and Quay.

Use the action in the following manner:

1. Create a GitHub Actions pipeline like you normally do.
1. Use any existing CI actions for compiling code, running unit tests, security scanning etc.
1. Place the final action in the pipeline as the "report image" action provided by Codefresh, copying the arguments and values from the GitHub Actions integration you set up in Codefresh.
1. When the pipeline completes execution, Codefresh retrieves the information on the image that was built and its metadata (essentially the same
data that Codefresh CI would send automatically).
1. View the image in Codefresh in the [Images dashboard]({{site.baseurl}}/docs/pipelines/images/)
 in Codefresh, and in any [GitOps deployment]({{site.baseurl}}/docs/deployment/applications-dashboard/) in which it is used.

 

### Codefresh-GitHub Action integration arguments
The table describes the arguments required for GitHub Action-Codefresh integration. 


 {: .table .table-bordered .table-hover}
| Argument  | Description     | Required/Optional/Default |
| ---------- |  -------- | ------------------------- |
| `CF_HOST`                      | The URL to the cluster with the Codefresh runtime to integrate with. If you have more than one runtime, select the runtime. Codefresh provides the cluster URL.  | Required  |
| `CF_API_KEY`                   | The API key to interact with Codefresh. Generate the key for the GitHub Action | Required  |
| `CF_CONTAINER_REGISTRY_INTEGRATION` | The registry integration to use.  | Optional  |
| `CF_JIRA_INTEGRATION`               | The Jira integration  to use for the image. Relevant only if Jira enrichment is required for the image. You can create a new When defined, `CF_ENRICHERS` must include `jira`.  | Optional  |
| `CF_IMAGE`                    | The name of the image to report to Codefresh.  | Required  |
| `CF_ENRICHERS`                | The type or types of enrichment information to retrieve and add to the image. By default, empty. When enrichment information is required, can be either `git`, `jira`, or both.  When an enricher type is defined, you must also define the required fields for the enrichment-type for Codefresh to retrieve information.  | Default  |
| `CF_WORKFLOW_URL`            | The reported URL of the workflow that builds the image.  | Optional  |
| `CF_WORKFLOW_NAME`           | The name assigned to the workflow that builds the image. When defined, the name is displayed in the Codefresh platform. Example, `CR` | Optional  |
| `CF_GITHUB_TOKEN`            | The token for Git integration.  | Required  |
| `CF_JIRA_PROJECT_PREFIX`     | Relevant only when `CF_ENRICHERS` includes `jira`. The Jira project prefix that identifies the ticket number for which to retrieve information.  | Optional  |
| `CF_JIRA_MESSAGE`            | Relevant only when `CF_ENRICHERS` includes `jira`. The Jira message that should be added to the image prefix.  | Optional  |



### Connect a GitHub Action in Codefresh

Add a GitHub Action to Codefresh with the required arguments. 
1. In the Codefresh UI, go to [Integrations](https://g.codefresh.io/2.0/account-settings/integrations){:target="\_blank"}.
1. Select **GitHub Actions**, and then click **Configure**.
1. Define the arguments for the GitHub Action. [Review GitHub Action arguments](#codefresh-github-action-integration-arguments). 
  * For the **CF_API_KEY**, click **Generate**. Note down the token generated.
  * To create a container registry integration for DockerHub if you don't have one, click **Create Container Registry Integration**, and then configure the settings. See [Docker Hub integration]({{site.baseurl}}/docs/integrations/docker-hub/).
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

> Copy the arguments and their values to the GitHub Action pipeline resource. See the example below for guidelines.

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
          CF_IMAGE: ${{ secrets.DOCKERHUB_USERNAME }}/report-image-github-action
        run: |
          docker build . --file Dockerfile --tag $CF_IMAGE && docker push $CF_IMAGE
          echo "Image should be accessible to your local machine (after docker login) by:"
          echo "docker pull $CF_IMAGE"
          docker pull $CF_IMAGE
          echo "On the next step, the report image would use the integration to pull information on the reported image, and using the specified enrichers."
      - name: report image by action
        with:
          # Specify cluster app-proxy
          CF_HOST: "${{ secrets.CF_RUNTIME_HOST }}"
          CF_API_KEY: ${{ secrets.USER_TOKEN }}
          # use image registry integration - the name of the integration used for pulling information on the image.
          CF_CONTAINER_REGISTRY_INTEGRATION: "docker"

          # report image 
          CF_IMAGE: ${{ secrets.DOCKERHUB_USERNAME }}/codefresh-report-image-github-action:example-reported-image
          # integration with
          CF_ENRICHERS: "jira, git"
          # use githug token
          CF_GITHUB_TOKEN: ${{ secrets.CF_GITHUB_TOKEN }}    
          # use jira integration - jira is the name of the integration created in codefresh argo platform.
          CF_JIRA_INTEGRATION: "jira" 
          CF_JIRA_MESSAGE: "A message with embedded issue ( i.e. CR-11027 ) that would be use query jira for the ticket "
          # a specified prefix for finding the jira issue 
          CF_JIRA_PROJECT_PREFIX: "CR"
        uses: codefresh-io/codefresh-report-image@0.0.61
        
{% endraw %}'
{% endhighlight yaml %}

### GitHub Action logs
When a GitHub Action is run, the 

### What to read next  
[Adding Git sources]({{site.baseurl}}/docs/runtime/git-sources/)

