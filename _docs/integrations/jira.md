---
title: "JIRA tracking"
description: "Enrich your images with information from JIRA "
group: integrations
toc: true
---

One of the major highlights of the Codefresh platform is the ability to automatically correlate 
software features with their deployment (where and when). While the software version of a component is easily identifiable, what is likely more interesting and important is to know the features included in a release.

Codefresh has native integration for Atlassian JIRA. This allows Codefresh to monitor a feature all the way from the ticket creation phase, up to the moment it is implemented and deployed to an environment.  

If are using other CI tools, adding a JIRA integration allows you to reference the integration by name in your GitHub Actions pipeline for example, and allow Codefresh to retrieve and display this enriched information as part of the deployment. See [Image enrichment overview]({{site.baseurl}}/docs/integrations/image-enrichment-overview/).

### Prerequisites

1. Get your JIRA instance credentials by following the [Atlassian documentation](https://support.atlassian.com/atlassian-account/docs/manage-api-tokens-for-your-atlassian-account/).
1. Note down the following as you will need to complete the integration with Codefresh:  

  * JIRA URL
  * JIRA username/email to be used for the integration
  * JIRA password/token created for this user


### Configure JIRA integration in Codefresh
Once you have set up a JIRA instance, configure the JIRA integration settings in Codefresh.  

1. In the Codefresh UI, go to [Integrations](https://g.codefresh.io/2.0/account-settings/integrations){:target="\_blank"}. 
1. Select **Atlassian JIRA**, and then click **Configure**.
1. Click **Add** on the top-right. 
1. Configure the JIRA integration settings:
  * Enter an **Integration name**. You can have multiple JIRA instances connected.
  * Use this integration for **All runtimes**, or specific **Selected runtimes**.
  * **JIRA Host**: The URL of your JIRA instance. For example, `https://<company>.atlassian.net`
  * **API Token**: The JIRA password/token you noted down when you created the JIRA instance.
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


### What to read next
[Shared runtime configuration]({{site.baseurl}}/docs/runtime/shared-configuration/)  
[Images]({{site.baseurl}}/docs/pipelines/images/)  
[Applications dashboard]({{site.baseurl}}/docs/deployment/applications-dashboard/)    
[Adding Git sources]({{site.baseurl}}/docs/runtime/git-sources/)  












