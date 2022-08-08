---
title: "Codefresh Classic"
description: ""
group: integration
toc: true
---

 Hosted GitOps can be used with any popular Continuous Integration (CI) solution, not just with Codefresh CI. Codefresh Classic is one of the external CI platform/tools that you can connect to Codefresh for image enrichment and reporting. 

For information on how to use the image reporting action in your Codefresh Classic pipeline and how to configure the integration, see [CI Integrations]({{site.baseurl}}/docs/integrations/ci-integrations/). 


### Codefresh Classic-Codefresh integration arguments
The table describes the arguments required to connect Codefresh Classic to Codefresh.  

{: .table .table-bordered .table-hover}
| Argument    | Description     | Required/Optional/Default |
| ----------  |  -------- | ------------------------- |
| `CF_RUNTIME_NAME`       | The runtime to use for the integration. If you have more than one runtime, select the runtime from the list. | Required  |
| `CF_API_KEY`            | The API key to authenticate the Codefresh Classic user to Codefresh. Generate the key for the integration.  | Required  |
| `CF_CONTAINER_REGISTRY_INTEGRATION` | The name of the container registry integration created in Codefresh to use for the report image action.  To create a container registry integration if you don't have one, click **Create Container Registry Integration**, and then configure the settings. | Optional  |
| `CF_JIRA_INTEGRATION`               | The name of the Jira integration created in Codefresh to use for the report image action. Relevant only if Jira enrichment is required for the image. If you don't have a Jira integration, click **Create Atlassian Jira Integration** and configure settings (see [Jira integration]({{site.baseurl}}/docs/integrations/jira/)).  | Optional  |
| `CF_IMAGE`                    | The path to the image to enrich and report to Codefresh.  | Required  |
| `CF_WORKFLOW_NAME`           | The name assigned to the workflow that builds the image. When defined, the name is displayed in the Codefresh platform. Example, `Staging step` | Optional  |
| `CF_GIT_BRANCH`              | The Git branch with the commit.  | Required  |
| `CF_GIT_REPO`                | The Git repository with the configuration and code used to build the image.  | Required  |
| `CF_GIT_PROVIDER`            | The Git provider for the integration, and can be either GiitHub, GitLab, or Bitbucket.  | Required  |
| `CF_GITHUB_TOKEN`            | The GitHub PAT (Personal Access Token) to use for Git integration. The token must have `repo` scope. See [Git tokens]({{site.baseurl}}/docs/reference/git-tokens/). | Required  |
| `CF_GITHUB_API_URL`          | The URL to the GitHub developer site.  | Required  |
| `CF_BITBUCKET_USERNAME`      | The username for the Bitbucket or the BitBucket Server (on-prem) account. | Required  |
| `CF_BITBUCKET_PASSWORD`      | The password for the Bitbucket or the BitBucket Server (on-prem) account. | Required  |
| `CF_BITBUCKET_HOST_URL`      | Relevant for Bitbucket Server accounts only. The URL address of your Bitbicket Server instance. Example, `https://bitbucket-server:7990`. | Required  |
|`CF_JIRA_PROJECT_PREFIX` | Relevant only when `CF_JIRA_INTEGRATION` is defined. The Jira project prefix that identifies the ticket number for which to retrieve information.| Required|
| `CF_JIRA_MESSAGE`            | Relevant only when `CF_JIRA_INTEGRATION` is defined. The Jira issue (the issue ID and the string describing the issue) for the `CF_JIRA_PROJECT_PREFIX` to associate with the image.  | Required  |
| `CF_JIRA_FAIL_ON_NOT_FOUND`            | Relevant only when `CF_JIRA_INTEGRATION` is defined. The report image action when the `CF_JIRA_MESSAGE` is not found. When set to `true`, the report image action is failed.  | Required  |

### Example of Codefresh Classic pipeline with report image action

{% highlight yaml %}
{% raw %}

reportImage:
  title: Report image to Codefresh CD
  type: codefresh-report-image
  working_directory: /code
  arguments:
     # The URL to the cluster with the Codefresh runtime to integrate with.
     CF_HOST: '[runtime-host-url]'

     # Codefresh API key !! Committing a plain text token is a security risk. We highly recommend using an encrypted secrets. !!
     # Documentation - https://codefresh.io/docs/docs/configure-ci-cd-pipeline/secrets-store/
     CF_API_KEY: ${{API_KEY}}

     # Image path to enrich
     CF_IMAGE: '[full image path here, including tag]'

     # Name of Container registry integration
     CF_CONTAINER_REGISTRY_INTEGRATION: 'v2'

     # The git branch which is related for the commit
     CF_GIT_BRANCH: '[name-of-your-git-branch]'

     # Name of Jira integration
     CF_JIRA_INTEGRATION: 'jira'

     # Jira project filter
     CF_JIRA_PROJECT_PREFIX: '[jira-project-prefix]'

     # String starting with the issue ID to associate with image
     CF_JIRA_MESSAGE: '[issue-id]'

{% endraw %}'
{% endhighlight yaml %}

### Codefresh Classic integration logs
View and analyze logs for GitHub Action workflows through the Logs tab. When a GitHub Action is run, it is added to the Logs tab.  
You can:  
* Filter by status or by date range to view a subset of actions
* Navigate to the build file in Codefresh Classic, and view the Codefresh report image step

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