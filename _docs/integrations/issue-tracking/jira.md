---
title: "Jira"
description: " "
canonical_url: 'https://codefresh.io/docs/docs/gitops-integrations/issue-tracking/jira/'
group: integrations
sub_group: issue-tracking
toc: true
---


Codefresh has native integration for Atlassian Jira, to both enrich images with information from Jira and report deployment information back to Jira. You can monitor a feature in Codefresh all the way, from the ticket creation phase to when it is implemented and deployed to an environment.  

Here are examples of the Images dashboard in Codefresh enriched with Jira information, and the Deployment tab in Jira with the reported information.  

  {% include 
   image.html 
   lightbox="true" 
   file="/images/integrations/jira/jira-image-dashboard.png" 
   url="/images/integrations/jira/jira-image-dashboard.png" 
   alt="Images dashboard: Summary with enriched image information" 
   caption="Images dashboard: Summary with enriched image information"
   max-width="60%" 
   %}


{% include 
	image.html 
	lightbox="true" 
	file="/images/integrations/jira/jira-deployment-info.png" 
	url="/images/integrations/jira/jira-deployment-info.png" 
	alt="Deployment information reported in Jira" 
	caption="Deployment information reported in Jira"
  max-width="60%" 
   %}

For general information on adding, editing, and deleting a Jira integration in Codefresh, see [Issue-tracking integrations]({{site.baseurl}}/docs/integrations/issue-tracking/).



### Prerequisites
Get your Jira instance credentials based on the type of integration selected:
  * Secret for deployment reporting
  * Auth Token for Jira enrichment

{::nomarkdown} 
<br>
{:/}

#### Secret (OAuth authentication) for deployment reporting
See [Create OAuth credentials in Jira cloud](https://support.atlassian.com/jira-cloud-administration/docs/integrate-with-self-hosted-tools-using-oauth/?permissionViolation=true){:target="\_blank"}.  

* In the Permissions list, make sure you select only **Deployments**.
* After you click Create, note down the generated **Client ID** and **Secret**.  
  	
	{% include 
	image.html 
	lightbox="true" 
	file="/images/integrations/jira/oauth-credentials.png" 
	url="/images/integrations/jira/oauth-credentials.png" 
	alt="Set up OAuth credentials in Jira Cloud" 
	caption="Set up OAuth credentials in Jira Cloud"
  max-width="40%" 
   %}

{::nomarkdown} 
<br>
{:/}

#### Auth Token (API token authentication) for Jira enrichment
See [Manage API tokens for your Atlassian account](https://support.atlassian.com/atlassian-account/docs/manage-api-tokens-for-your-atlassian-account/){:target="\_blank"}.  

Note down the following as you will need them to complete the integration with Codefresh:  
  * Jira URL
  * Jira email 
  * Jira token



### Jira integration settings in Codefresh

The table describes the arguments required to integrate Jira in Codefresh, both for reporting deployment information back to Jira and for enriching images with Jira information. 

{: .table .table-bordered .table-hover}
| Setting    | Description     | 
| ----------  |  -------- | 
|**Integration name**       | A friendly name for the integration. This is the name you will reference in the third-party CI platform/tool. |
| **All Runtimes/Selected Runtimes**   | {::nomarkdown} The runtimes in the account with which to share the integration resource. <br>The integration resource is created in the Git repository with the shared configuration, within <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">resources</span>. The exact location depends on whether the integration is shared with all or specific runtimes: <br><ul><li>All runtimes: Created in <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">resources/all-runtimes-all-clusters/</span></li><li>Selected runtimes: Created in <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">resources/runtimes/<runtime-name></span></li></ul> You can reference the Docker Hub integration in the CI tool. {:/}|
|**Report deployment information to Jira** | For deployment reporting only. When enabled, writes back to Jira when this integration is referenced in your CI pipeline. |
|**Authentication**| {::nomarkdown} <ul><li><b>Secret</b> for OAuth authentication, for reporting deployment information to Jira.<ul><li><b>Jira Host</b>: The URL of your Jira instance. For example, {% raw %}`https://<company>.atlassian.net`{% endraw %}</li><li><b>Client ID</b>: The ID that was generated when you created OAuth credentials in Jira Cloud. </li><li><b>Secret</b>: The secret that was generated when you created OAuth credentials in Jira Cloud. </li></ul><li><b>Auth Token</b> for API token authentication: Supported for enriching image with Jira information in Codefresh.<ul><li><b>Jira Host</b>: The URL of your Jira instance. For example, {% raw %}`https://<company>.atlassian.net`{% endraw %}</li><li><b>API Token</b>The Jira API token for the Jira instance.</li><li><b>API Email</b>: The email for the API token.</li></ul></li></ul> {:/} |
| **Test connection**       | Click to verify that you can connect to the specified instance before you commit changes. |



  {% include 
	image.html 
	lightbox="true" 
	file="/images/integrations/jira/jira-deployment-report-int-settings.png" 
	url="/images/integrations/jira/jira-deployment-report-int-settings.png" 
	alt="Integration settings for reporting deployment information to Jira" 
	caption="Integration settings for reporting deployment information to Jira"
  max-width="80%" 
%}

  {% include 
	image.html 
	lightbox="true" 
	file="/images/integrations/jira/jira-enrichment-int-settings.png" 
	url="/images/integrations/jira/jira-enrichment-int-settings.png" 
	alt="Integration settings for image enrichment with Jira information" 
	caption="Integration settings for image enrichment with Jira information"
  max-width="80%" 
%}
 

### Using Jira integration in pipelines
In your CI pipeline, configure the Jira integration in Codefresh, and reference the Jira integration by name.  
Codefresh uses the Secret Key stored in the runtime cluster to securely access Jira and retrieve the information. 

### Related articles
[Shared configuration repo]({{site.baseurl}}/docs/reference/shared-configuration/)  
[Image enrichment with integrations]({{site.baseurl}}/docs/integrations/image-enrichment-overview/)
[CI integrations]({{site.baseurl}}/docs/integrations/ci-integrations/)  
[Container registry integrations]({{site.baseurl}}/docs/integrations/container-registries/)  
