---
title: "DORA metrics"
description: "Get insights for your deployments"
group: reporting
toc: true
---

DevOps is a collaboration paradigm, that is sometimes mistaken for being too abstract or too generic. In an effort to quantify the benefits of adopting DevOps,
[Dora Research](https://www.devops-research.com/research.html#capabilities) (acquired by Google in 2018), has introduced four key metrics that
define specific goals for improving the software lifecycle in companies interested in adopting DevOps.

DORA ([read more](https://cloud.google.com/blog/products/devops-sre/using-the-four-keys-to-measure-your-devops-performance)) measures these metrics:

* Deployment Frequency: How often an organization successfully releases to production
* Lead Time for Changes: The length of time for a commit to be deployed into production
* Change Failure Rate: The percentage of deployments causing a failure in production
* Time to Restore Service: The length of time for an organization to recover from a failure in production

### DORA metrics in Codefresh

Monitoring these metrics can help identify delivery issues in your organization by detecting bottlenecks among teams, and optimize your workflows, at technical or organizational levels.  

Codefresh offers support for DORA metrics out of the box.  
* In the Codefresh UI, go to [DORA metrics](https://g.codefresh.io/2.0/dora-dashboard/dora){:target="\_blank"}. 

{% include
image.html
lightbox="true"
file="/images/reporting/dora-metrics.png"
url="/images/reporting/dora-metrics.png"
alt="DORA metrics report"
caption="DORA metrics report"
max-width="100%"
%}

### Filters

Use filters to define the exact subset of applications you are interested in:

* Runtimes: Limit metrics to applications only from selected runtimes 
* Clusters: Limit metrics to applications deployed to selected cluster
* Applications: Show selected applications 
* Labels: Show only applications that match the selected labels (key/value) 
* Time: Limit metrics to a specific time period

All filtering components support auto-complete and are multi-select. 

### Metric totals

Get key metrics for the selected subset of applications through the Totals bar.  

As the title indicates, the Totals bar shows the total numbers for the selected date range:

* Deployments
* Rollbacks
* Commits/Pull Requests
* Failure Rate: The number of failed deployments divided by the total number of deployments

### Metric graphs
The graphs show the performance of the same key metrics for the selected date range. 

Select granularity for each graph:

* Daily 
* Weekly
* Monthly



* **Deployment Frequency**  
  The frequency of deployments of any kind, successful or failed, for all applications in filter. Deployment is considered an Argo CD sync where there was a change. The X-axis charts the time based on the granularity, and the Y-axis charts the number of deployments. The number shown on the top right is the average deployment frequency based on granularity.  

* **Change failure rate**  
  The failure or rollback rate in percentage for deployments. Derived by dividing the failed/rollback deployments by the total number of deployments. Failed deployments are those Argo CD deployments that lead to a sync state of Degraded. The X-axis charts the time based on the granularity, and the Y-axis charts the failure rate. The number shown on the top right is the average failure rate based on granularity.  

* **Lead Time for Changes**  
  The average number of days from the first commit for a pull request until the deployment date for the same pull request. The X-axis charts the time based on the granularity, and the Y-axis charts the time in minutes until the deployment. The number shown on the top right is the average number of days for a commit to reach production.  

* **Time to Restore Service**  
  The average number of hours between the change in status to Degraded or Unhealthy after deployment, and back to Healthy. The X-axis charts the time based on the granularity, and the Y-axis charts  the time in hours. The number shown on the top right is the average number of hours between the previous deployment and rollback for the same application.

## What to read next  
[Codefresh architecture]({{site.baseurl}}/docs/getting-started/architecture/)  
[Applications dashboard]({{site.baseurl}}/docs/deployment/applications-dashboard/)

