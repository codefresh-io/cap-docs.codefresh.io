---
title: "CI integrations"
description: ""
group: integration
toc: true
---

Codefresh Hosted GitOps can be used with any popular Continuous Integration (CI) solution, not just with Codefresh CI.

You can connect any external CI solution to Codefresh, such as GitHub Actions for example, to take care of common CI tasks such as building/testing/scanning source code, with Codefresh Hosted GitOps still responsible for the deployment, including image enrichment and reporting.  
See [Image enrichment with integrations]({{site.baseurl}}/docs/integrations/image-enrichment-overview/).

### Codefresh image reporting and enrichment action
To support the integration between Codefresh and third-party CI tools, we have created dedicated actions for each tool in the Codefresh Marketplace. The actions combine image enrichment and reporting through integrations with Jira, and container registries.

[Codefresh marketplace](https://github.com/marketplace/actions/codefresh-report-image){:target="\_blank"}. 


Use the action in the following manner:

1. Create your pipeline with your CI platform/tool as you usually do.
1. Use any existing CI actions for compiling code, running unit tests, security scanning etc.
1. Place the final action in the pipeline as the "report image" action provided by Codefresh.  
  See:  
  ??
  
1. When the pipeline completes execution, Codefresh retrieves the information on the image that was built and its metadata (essentially the same
data that Codefresh CI would send automatically).
1. View the image in Codefresh in the [Images dashboard]({{site.baseurl}}/docs/deployment/images/), and in any [GitOps deployment]({{site.baseurl}}/docs/deployment/applications-dashboard/) in which it is used.

### Connect a CI platform/tool to Codefresh




### GtiHub Actions arguments


### Jenkins arguments


### Codefresh Classic arguments

