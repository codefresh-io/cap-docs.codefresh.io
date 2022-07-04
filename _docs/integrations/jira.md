---
title: "Jira tracking"
description: "Enrich your images with information from Jira "
group: integrations
toc: true
---

One of the major highlights of the Codefresh platform is the ability to automatically correlate 
software features with their deployment (where and when). While the software version of a component is easily identifiable, what is likely more interesting and important is to know the features included in a release.

Codefresh has native integration for Atlassian Jira. This allows Codefresh to monitor a feature all the way from the ticket creation phase, up to when it is implemented and deployed to an environment.  

Adding a Jira integration allows you to reference the integration in your pipelines (Codefresh or external), by the name of the integration account, instead of adding explicit credentials. See [Image enrichment with integrations]({{site.baseurl}}/docs/integrations/image-enrichment-overview/).


### Prerequisites

1. Get your Jira instance credentials by following the [Atlassian documentation](https://support.atlassian.com/atlassian-account/docs/manage-api-tokens-for-your-atlassian-account/).
1. Note down the following as you will need them to complete the integration with Codefresh:  
  * Jira URL
  * Jira username/email to be used for the integration
  * Jira password/token created for this user


### Configure Jira integration in Codefresh
Once you have set up a Jira instance, configure the Jira integration settings in Codefresh.  

1. In the Codefresh UI, go to [Integrations](https://g.codefresh.io/2.0/account-settings/integrations){:target="\_blank"}. 
1. Select **Atlassian Jira**, and then click **Configure**.
1. Click **Add** on the top-right. 
1. Configure the Jira integration settings:
  * Enter an **Integration name**. You can have multiple Jira instances connected.
  * Use this integration for **All runtimes**, or specific **Selected runtimes**.
  * **Jira Host**: The URL of your Jira instance. For example, `https://<company>.atlassian.net`
  * **API Token**: The Jira password/token you noted down when you created the Jira instance.
  * **API Email**: The email for the API token.

  {% include 
	image.html 
	lightbox="true" 
	file="/images/integrations/jira/jira-int-settings.png" 
	url="/images/integrations/jira/jira-int-settings.png" 
	alt="JIRA integration in Codefresh" 
	caption="JIRA integration in Codefresh"
  max-width="50%" 
%}
{:start="5"}
1. To confirm, click **Commit**.
  It may take a few moments for the changes to be synced to the cluster before the integration account appears in the list.
  

### Integration resource in shared runtime configuration
The integration resource is created in the shared runtime configuration Git repository, under `resources`.
The exact location depends on whether the integration is shared with _all_ or _specific_ runtimes:  
* All runtimes: Created in `resources/all-runtimes-all-clusters/`
* Selected runtimes: Created in `resources/runtimes/<runtime-name>/`

### Using Jira integration in pipelines

The usage is slightly different for an external CI versus Codefresh-native pipelines, but in both cases you can replace explicit Jira credentials with just the name of the Jira integration to use. Codefresh uses the Secret Key stored in the runtime cluster to securely access Jira and retrieve the information. 

* For pipelines based on GitHub Actions:  
  Configure the Jira integration in Codefresh, and then connect your GitHub Action to Codefresh, as described in [GitHub Action integration]({{site.baseurl}}/docs/integrations/github-actions/).

* For Codefresh-native pipelines: 
  If you are using the [Codefresh Jira image enrichment template](https://github.com/codefresh-io/argo-hub/blob/main/workflows/codefresh-csdp/versions/1.0.0/docs/image-enricher-jira-info.md) in your pipeline:  
  Replace: `JIRA_HOST_URL`, `JIRA_API_TOKEN_SECRET`, `JIRA_API_TOKEN_SECRET_KEY` and `IRA_EMAIL_SECRET_KEY` with `CF_JIRA_INTEGRATION` and the name of the integration added in Codefresh as the value. For example, `jira`.

      

### What to read next
[Shared runtime configuration]({{site.baseurl}}/docs/runtime/shared-configuration/)  
[Images]({{site.baseurl}}/docs/pipelines/images/)  
[Applications dashboard]({{site.baseurl}}/docs/deployment/applications-dashboard/)    
[Adding Git sources]({{site.baseurl}}/docs/runtime/git-sources/)  












