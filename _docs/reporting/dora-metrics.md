---
title: "DORA metrics"
description: "Get insights for your deployments"
group: reporting
toc: true
---

DevOps is a collaboration paradigm and sometimes it is mistaken for being too
abstract or too generic. In an effort to quantify the benefits of adopting DevOps,
[Dora Research](https://www.devops-research.com/research.html#capabilities) (acquired by Google in 2018) has introduced 4 key metrics that
define specific goals for improving the software lifecycle in companies that are
interested in adopting DevOps.

DORA measures these metrics ([read more](https://cloud.google.com/blog/products/devops-sre/using-the-four-keys-to-measure-your-devops-performance))


* Deployment Frequency — How often an organization successfully releases to production
* Lead Time for Changes — The amount of time it takes a commit to get into production
* Change Failure Rate — The percentage of deployments causing a failure in production
* Time to Restore Service — How long it takes an organization to recover from a failure in production

## DORA metrics in Codefresh

Monitoring these metrics can help you identify delivery issues in your organization by detecting bottlenecks among teams, and identifying to optimize your workflows, at technical or organizational levels.

Codefresh offers support for DORA metrics out of the box. You can visit your metric dashboard by selecting *DORA metrics* from the left sidebar

{% include
image.html
lightbox="true"
file="/images/reporting/dora-metrics.png"
url="/images/reporting/dora-metrics.png"
alt="DORA metrics report"
caption="DORA metrics report"
max-width="100%"
%}

## Filters

Use filters to define the exact subset of applications you are interested in:


* Runtimes: Limit metrics to Applications only from selected runtimes 
* Clusters: Limit metrics to Applications deployed to selected cluster
* Applications: Show selected applications 
* Labels: Show only Applications that have the matching labels (key/value) 
* Time: Limit metrics to a specific time period

All filtering components support auto-complete and are multi-select. 

## Quick metrics

The middle part of the screen shows a quick glance over key metrics for the selected subset of applications.

The results shown are :

* Deployments -  number of total deployments
* Rollbacks - number of rollbacks that took place
* Commits/Pull Requests - number of pull requests
* Failure Rate - Division of Rollbacks / Deployments

## Graphs


First you can select granularity of each graph by choosing the following options:

* Daily 
* Weekly
* Monthly


The graphs shown are:

* **Deployment Frequency**.  Deployments of any kind, successful or failed for all applications in filter. Deployment is considered an Argo CD sync where there was a change. X-axis is time, Y axis is number of deployments. The number shown on the top right is Average deployment frequency based on granularity
* **Change failure rate**. Display a chart of how often deployments fail/rollback divided by total deployments. Failed deployments are those Argo CD deployments that lead to a sync state of "degraded". X-axis is time, Y-axis is failure rate. The number shown on the top right is Average failure rate. based on granularity
* **Lead Time for Changes**. Display a chart of Average Number of days from first commit in pull request until the deployment date for that pull request. X-axis is time, Y-axis is minutes. The number shown on the top right is Average number of days for a commit to reach production
* **Lead Time for Changes**. Display a chart of the number of hours between a rollback and the previous deployment for the same application. X-axis is time, Y-axis is hours. The number shown on the top right is Average number of hours fbetween rollback and previous deployment for the same application

## What to read next

* [Codefresh architecture](({{site.baseurl}}/docs/getting-started/architecture/))
* [Applications dashboard](({{site.baseurl}}/docs/deployment/applications-dashboard//))

