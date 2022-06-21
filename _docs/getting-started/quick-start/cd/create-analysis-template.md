---
title: "Create application specifications"
description: ""
group: getting-started
sub-group: quick-start
toc: true
---

Create an `AnalysisTemplate` specification to validate that your changes conforms to the requirements before deployment.  

For the quick start, you'll create the `background-analysis` analysis template, that interfaces with Prometheus to validate metrics. 
> Argo Rollouts supports several third-party metric providers, such as Prometheus, Datadog, Wavefront, and more. Go to the [Analysis section in Argo Rollouts](https://argoproj.github.io/argo-rollouts/){:target="\_blank"}. 


The template queries a Prometheus server a total of four times (`count: 4`) at 5 second intervals (`interval: 5s`). The metric value in the response must be higher than 100 (`successCondition: result[0] >= 100`). If the metric value is less than 100 more than once (`failureLimit: 1`), the analysis is considered Failed, and the Rollout aborted. Traffic is routed back to the stable version of the application.

* In the Git repository create the `AnalysisTemplate.yaml` file, as in the example below.


```yaml
apiVersion: argoproj.io/v1alpha1
kind: AnalysisTemplate
metadata:
  name: background-analysis
spec:
  metrics:
    - name: prometheus-metric
      count: 4
      interval: 5s
      successCondition: result[0] >= 100
      failureLimit: 1
      provider:
        prometheus:
          address: http://a95910c83807a4089a2458554bf5c21e-1864259807.us-east-1.elb.amazonaws.com:9090
          query: |
            sum(argocd_app_reconcile_sum)
```