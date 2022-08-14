---
title: "Image enrichment with integrations"
description: ""
group: integration
toc: true
---




Image enrichment is a crucial part of the CI/CD process, adding to the quality of deployments. Image enrichment exposes metadata such as feature requests, pull requests, and logs as part of the application's deployment, for visibility into all aspects of the deployment, making it easier to track actions and identify root cause of failures.  

If you have your CI tools and our Hosted GitOps, you can still enrich and report images to the Codefresh platform with no disruptions to existing CI processes and flows.  

Codefresh has new report images templates, optimized to work with external CI tools/plaforms for creating pipelines and workflows. Add integration accounts in Codefresh to tools such as Jira, Docker Hub and Quay, and then connect your CI tool with Codefresh for image enrichment and reporting.  

Codefresh  popular CI tools including:
* GitHub Actions
* Jenkins
* Codefresh Classic [report image template](https://github.com/marketplace/actions/codefresh-report-image){:target="\_blank"} that combines image enrichment and reporting. 


### CI integration flow for image enrichment
 
Integrate Codefresh with your CI platform/tool account with a unique name per integration account. 

#### 1. Add/configure integration

Add/configure the integration account for the third-party tools. You can set up multiple integration accounts for the same tool.  
When you add an integration, Codefresh creates a Sealed Secret with the integration credentials, and a ConfigMap that references the secret.  

See:  
* Issue tracking  
  [JIRA]({{site.baseurl}}/docs/integrations/jira/) 
 
* Container registries  
  [DockerHub]({{site.baseurl}}/docs/integrations/dockerhub/)  
  [JFrog Artifactory]({{site.baseurl}}/docs/integrations/jfrog/)  
  [Quay]({{site.baseurl}}/docs/integrations/quay/)  

We are working on supporting integrations for more tools. Stay tuned for the release announcements.  
For image enrichment with a tool that is as yet unsupported, you must define the explicit credentials. 
   
#### 2. Connect CI platform/tool to Codefresh

Connect a CI platform/tool to Codefresh with an API token for the runtime cluster, the integration accounts, and image information for enrichment and reporting. 

See:  
[Codefresh Classic]({{site.baseurl}}/docs/integrations/codefresh-classic/)  
[GitHub Actions]({{site.baseurl}}/docs/integrations/github-actions/)
[Jenkins]({{site.baseurl}}/docs/integrations/jenkins/)


#### 3. Add the enrichment step for the CI platform/tool to your GitHub Actions pipeline 

Finally, add the enrichment step to your CI pipeline with the API token and integration information. Codefresh uses the integration name to get the corresponding Sealed Secret to securely access and retrieve the information for image enrichment.  


#### 4. View enriched image information
Once deployed, view enriched information in the dashboards:  
* [Images](https://g.codefresh.io/2.0/images){:target="\_blank"}  
* [Applications dashboard](https://g.codefresh.io/2.0/applications-dashboard){:target="\_blank"}  


View:  

* Commit information as well as committer
* Links to build and deployment pipelines
* PRs included in the deployment
* Jira issues, status and details for each deployment


### Related articles
[Images]({{site.baseurl}}/docs/deployment/images/)  
[Applications dashboard]({{site.baseurl}}/docs/deployment/applications-dashboard/) 

