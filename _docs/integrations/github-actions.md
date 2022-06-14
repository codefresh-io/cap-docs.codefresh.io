---
title: "GitHub Actions"
description: "Connect your GitHub Actions pipelines to Codefresh"
group: integrations
toc: true
---

Codefresh GitOps can be used with any popular Continuous Integration (CI) solution, not just with Codefresh CI.

You can connect any external solution to Codefresh, and have it take care of common CI tasks such as building/testing/scanning source code, while Codefresh GitOps is still responsible for the deployment.

### The Codefresh GitHub Action

To accommodate the integration between GitHub Actions and Codefresh, we have created a dedicated action that you can find at [https://github.com/marketplace/actions/csdp-report-image](https://github.com/marketplace/actions/csdp-report-image).

Use the action in the following manner:

1. Create a GitHub Actions pipeline like you normally do
1. Use any existing CI actions for compiling code, running unit tests, security scanning etc
1. Place the last action in the pipeline as the "report image" action provided by Codefresh
1. When the pipeline completes execution, GitHub Actions sends all required information to Codefresh regarding the image that was built and its metadata (essentially the same
data that Codefresh CI would send automatically).
1. The image will now be available in the [image dashboard]({{site.baseurl}}/docs/pipelines/images/)
 in Codefresh and is ready to be used with any [GitOps deployment]({{site.baseurl}}/docs/deployment/applications-dashboard/).

The action expects the following arguments:


 {: .table .table-bordered .table-hover}
| Argument  | Description     | Required/Optional/Default |
| ---------- |  -------- | ------------------------- |
| `CF_API_KEY`         | API key for interacting with Codefresh  | Required  |
| `CF_IMAGE`         | Name of image that was built  | Required  |
| `CF_HOST`         | Codefresh installation. Defaults to your company URL  | Default  |
| `CF_VERBOSE`         | Enable verbose output. Default is `false`  | Default  |
| `CF_ENRICHERS`         | Array with `git` or `jira` or both. Default is empty  | Default  |
| `CF_CONTAINER_REGISTRY_INTEGRATION`         | which registry integration to use  | Optional  |
| `CF_WORKFLOW_URL`         | Which Codefresh workflow to link this image to   | Optional  |
| `CF_LOGS_URL`         | Which logs view to link this image to   | Optional  |
| `CF_GIT_SHA`         | Which Git SHA was used to build this image  | Optional  |
| `CF_GIT_REPO`         | Which repo contains the source code  | Optional  |
| `CF_GIT_BRANCH`         | Which repo branch contains the source code   | Optional  |
| `CF_JIRA_PROJECT_PREFIX`         | Which JIRA project this image should be assigned to  | Optional  |
| `CF_JIRA_MESSAGE`         | String that contains PREFIX-ISSUE_ID for JIRA   | Optional  |
| `CF_JIRA_INTEGRATION`         | Which JIRA integration will be used for this image  | Optional  |
| `CF_JIRA_FAIL_ON_NOT_FOUND `         | Fail if Jira ticket is not found  | Optional  |

Most of the arguments have default values that will match the most common scenarios.
The most important arguments that you need to fill manually in your GitHub Action workflow are:

* `CF_API_KEY`
* `CF_IMAGE`
* `CF_ENRICHERS`

### Codefresh API Key

1. In the Codefresh UI, go to [Integrations](https://g.codefresh.io/2.0/account-settings/integrations){:target="\_blank"}.
1. Select **GitHub Actions**, and then click **Configure**.
1. In the row with the **CF_API_KEY**, click **Generate** (first row).  
  A token is generated. Make sure to note down the token as it will only appear once.
1. Enter this token in GitHub Actions [as a secret](https://docs.github.com/en/actions/security-guides/encrypted-secrets) with the name `CF_API_KEY`.  
  Now you can reference it in all GitHub pipelines as you would any other secret.

### GitHub Actions pipeline example

Here is an example pipeline that uses GitHub Actions to build a container image, and the special Codefresh action to pass the resulting image to Codefresh.


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
          "CF_GITHUB_TOKEN": ${{ secrets.G_TOKEN }}
          "CF_GIT_PROVIDER": "github"
          "CF_GIT_REPO": "codefresh-io/example-github-action-use-csdp-report-image"
          "CF_GIT_BRANCH": "feature"

          "CF_JIRA_API_TOKEN": ${{ secrets.JIRA_TOKEN }}
          "CF_JIRA_HOST_URL": "https://my-jira-instance.atlassian.net"
          "CF_JIRA_EMAIL": "username@acme.inc"
          "CF_JIRA_MESSAGE": "implements PRO-13427"
          "CF_JIRA_PROJECT_PREFIX": "PRO"
        uses: codefresh-io/csdp-report-image@0.0.40
{% endraw %}'
{% endhighlight yaml %}


## What to read next  
[Adding Git sources]({{site.baseurl}}/docs/runtime/git-sources/)










