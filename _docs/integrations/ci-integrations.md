---
title: "CI integrations"
description: ""
group: integrations
toc: true
---

Use Codefresh's Hosted GitOps with any popular Continuous Integration (CI) solution, not just with Codefresh CI.

You can connect a third-party CI solution to Codefresh, such as GitHub Actions for example, to take care of common CI tasks such as building/testing/scanning source code, and have Codefresh Hosted GitOps still responsible for the deployment, including image enrichment and reporting.  
See [Image enrichment with integrations]({{site.baseurl}}/docs/integrations/image-enrichment-overview/).

### Codefresh image reporting and enrichment action
To support the integration between Codefresh and third-party CI platforms and tools, we have created dedicated actions for supported CI tools in the Codefresh Marketplace. These actions combine image enrichment and reporting through integrations with issue tracking and container registry tools. 

>You can also configure the integration directly in the Codefresh UI, as described in [Connect a CI platform/tool to Codefresh](#connect-a-ci-platform-tool-to-Codefresh).


Use the action as follows:

1. Create your pipeline with your CI platform/tool as you usually do.
1. Use existing CI actions for compiling code, running unit tests, security scanning etc.
1. Place the final action in the pipeline as the "report image" action provided by Codefresh.  
  See:  
 [GitHub Action Codefresh report image](https://github.com/marketplace/actions/codefresh-report-image){:target="\_blank"}.  
 [Codefresh Classic Codefresh report image](https://codefresh.io/steps/step/codefresh-report-image){:target="\_blank"}.    
1. When the pipeline completes execution, Codefresh retrieves the information on the image that was built and its metadata through the integration names specified (essentially the same data that Codefresh CI would send automatically).
1. View the image in Codefresh's [Images dashboard]({{site.baseurl}}/docs/deployment/images/), and in any [application]({{site.baseurl}}/docs/deployment/applications-dashboard/) in which it is used.

### Connect a third-party CI platform/tool to Codefresh
Connecting the CI platform/tool to Codefresh from the UI includes configuring the required arguments, and then generating and copying the YAML manifest for the report image to your pipeline.  

1. In the Codefresh UI, go to [Integrations](https://g.codefresh.io/2.0/account-settings/integrations){:target="\_blank"}.
1. Filter by **CI tools**, select the CI tool, and click **Add**.
1. Define the arguments for the CI tool:  
  [Codefresh Classic]({{site.baseurl}}/docs/integrations/codefresh-classic/)  
  [GitHub Action]({{site.baseurl}}/docs/integrations/github-action/)  
  [Jenkins]({{site.baseurl}}/docs/integrations/jenkins/)
1. To generate a YAML snippet with the arguments, on the top-right, click **Generate Manifest**. 
1. In the generated manifest, add fields and values, as needed.
1. To copy the YAML manifest, click **Copy**.

{% include image.html 
lightbox="true" 
file="/images/integrations/classic/classic-manifest.png" 
url="/images/integrationsclassic/classic-manifest.png"
alt="Example of manifest generated for Codefresh Classic"
caption="Example of manifest generated for Codefresh Classic"
max-width="50%"
%}

{:start="6"}
1. Paste it as the last step in your CI pipeline.


### Related articles
[Container registry integrations]({{site.baseurl}}/docs/integrations/container-registries/)  
[Jira integrations]({{site.baseurl}}/docs/integrations/jira/)  






