---
title: "Image enrichment overview"
description: ""
group: integration
toc: true
---




Image enrichment is a crucial part of the CI/CD process, adding to the quality of deployments. Image enrichment exposes metadata such as feature requests, pull requests, and logs as part of the application's deployment, providing a holistic view of the deployment, and making it easier to track actions and identify root cause of failures. 

With Codefresh you can enjoy the full benefits of GitOps, with and without continuous integration (CI). Integrating Codefresh with the CI tool or platform you use brings the enrichment information for the image. The integration mechanism is simple and easy to configure. The key benefit is that the mechanism relies on the integration name to retrieve the information.  

Existing pipelines in Codefresh are not affected by the new integrations, avoiding the need to migrate or convert pipelines.


### How does CI Connect work?
 
Integrate Codefresh with your CI platform/tool account with a unique name per integration account. 

**Add/configure the integration**  

Add/configure the integration account for the third-party platform/tool. You can set up multiple integration accounts for the same CI platform or tool.  

Codefresh supports the following CI platforms/tools:

* [JIRA]({{site.baseurl}}/docs/integration/jira/)  
* [DockerHub]({{site.baseurl}}/docs/integration/dockerhub/)
* [Quay]({{site.baseurl}}/docs/integration/quay/)  
* [GitHub Actions]({{site.baseurl}}/docs/integration/github-actions/)

We are working on supporting integrations for more CI platforms/tools. Stay tuned for the release announcements.  
   
**Define the enrichment step in your pipeline**  

In the enrichment step, specify the names of one or more integration accounts, as needed. 
For example, if you have JIRA integration in Codefresh, in the GitHub Action-based pipeline, the enrichment step in your pipeline would include the following:

```yaml
"CF_JIRA_INTEGRATION": "codefresh-jira"
"CF_JIRA_MESSAGE": "wip CR-11027"
"CF_JIRA_PROJECT_PREFIX": "CR"
```
`CF_JIRA_INTEGRATION` defines the integration account, `codefresh-jira`.  

Codefresh uses the integration name to identify and retrieve information for `wip CR-11027` and display it as part of the deployment information. There is no need to enter the credentials for the JIRA integration account in the enrichment. 

**View enriched information**  

* For hybrid runtimes, go to [Images](https://g.codefresh.io/2.0/images){:target="\_blank"}.
* For both hybrid and hosted runtimes, go to the [Applications dashboard](https://g.codefresh.io/2.0/applications-dashboard?sort=desc-lastUpdated){:target="\_blank"}. 

You can view:

* Commit information as well as committer
* Links to build and deployment pipelines
* PRs included in this deployment
* Jira issues, status and details for each deployment

### Example of GitHub Action pipeline with image enrichment 
This is an example of a pipeline managed by a GitHub Action that includes the Codefresh step for reporting the image.  
As you can see, `CF_CONTAINER_REGISTRY_INTEGRATION` references Quay by the integration name, `quay` in this example. `CF_JIRA_INTEGRATION` also references the required JIRA account by the integration name, `jira` in the example. Both references do not require explicit credentials.  
In contrast, for Git information, `CF_GITHUB_TOKEN` must be defined.

```yaml
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
          CF_HOST: "https://eti-demo.pipeline-team.cf-cd.com"
          CF_VERBOSE: "1"
          CF_API_KEY: ${{ secrets.ETI_TOKEN }}
          CF_CONTAINER_REGISTRY_INTEGRATION: "quay"
          CF_GITHUB_TOKEN: ${{ secrets.G_TOKEN }}
          CF_GIT_PROVIDER: "github"
          CF_GIT_REPO: "codefresh-io/example-github-action-use-csdp-report-image"
          CF_GIT_BRANCH: "feature"
          CF_JIRA_INTEGRATION: "jira"
          CF_JIRA_MESSAGE: "wip CR-11027"
          CF_JIRA_PROJECT_PREFIX: "CR"
        uses: codefresh-io/csdp-report-image@0.0.45
```
### What to read next
[Images]({{site.baseurl}}/docs/pipelines/images/)  
[Applications dashboard]({{site.baseurl}}/docs/deployment/applications-dashboard/) 

