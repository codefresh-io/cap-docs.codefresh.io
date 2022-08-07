---
title: "Jenkins"
description: ""
group: integration
toc: true
---

 Hosted GitOps can be used with any popular Continuous Integration (CI) solution, not just with Codefresh CI. Jenkins is one of the external CI platform/tools that you can connect to Codefresh for image enrichment and reporting. 

For information on how to use the image reporting action in your Jenkins pipeline and how to configure the integration in Codefresg, see XREF. 


### Jenkins-Codefresh integration arguments
The table describes the arguments to connect Codefresh Classic to Codefresh.  

{: .table .table-bordered .table-hover}
| Argument    | Description     | Required/Optional/Default |
| ----------  |  -------- | ------------------------- |
| `CF_RUNTIME_NAME`       | The runtime to use for the integration. If you have more than one runtime, select the runtime from the list. | Required  |
| `CF_API_KEY`            | The API key to authenticate the GitHub Actions user to Codefresh. Generate the key for the integration. | Required  |
| `CF_CONTAINER_REGISTRY_INTEGRATION` | The name of the container registry integration created in Codefresh to use for the report image action.  | Optional  |
| `CF_JIRA_INTEGRATION`               | The name of the Jira integration created in Codefresh to use for the report image action. Relevant only if Jira enrichment is required for the image. If you don't have a Jira integration, create a new integration (see [Jira integration]({{site.baseurl}}/docs/integrations/jira/)).  | Optional  |
| `CF_IMAGE`                    | The path to the image to enrich and report to Codefresh.  | Required  |
| `CF_GIT_BRANCH`              | The Git branch with the commit.  | Required  |
| `CF_GIT_REPO`                | The Git repository with the configuration and code used to build the image.  | Required  |
| `CF_GIT_PROVIDER`            | The Git provider for the integration.  | Required  |
| `CF_GITHUB_TOKEN`            | The GitHub PAT (Personal Access Token) to use for Git integration. The token must have `repo` scope. See [Git tokens]({{site.baseurl}}/docs/reference/git-tokens/). | Required  |
| `CF_GITHUB_API_URL`          | The URL to the GitHub developer site.  | Required  |
| `CF_GITLAB_TOKEN`      | The  | Required  |
| `CF_GITLAB_HOST_URL`      | The  | Optional  |
| `CF_BITBUCKET_USERNAME`      | The  | Required  |
| `CF_BITBUCKET_PASSWORD`      | The  | Required  |
| `CF_BITBUCKET_HOST_URL`      | The  | Required  |
| `CF_JIRA_PROJECT_PREFIX`     | Relevant only when `CF_JIRA_INTEGRATION` is defined. The Jira project prefix that identifies the ticket number for which to retrieve information.  | Required  |
| `CF_JIRA_MESSAGE`            | Relevant only when `CF_JIRA_INTEGRATION` is defined. The Jira issue (the issue ID and the string describing the issue) for the `CF_JIRA_PROJECT_PREFIX` to associate with the image.  | Required  |
| `CF_JIRA_FAIL_ON_NOT_FOUND`            | Relevant only when `CF_JIRA_INTEGRATION` is defined. The report image action when the `CF_JIRA_MESSAGE` is not found. When set to `true`, the report image action is failed.  | Required  |

### Example of Jenkins pipeline with report image action


### Jenkins integration logs