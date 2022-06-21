---
title: "Create application specifications"
description: ""
group: getting-started
sub-group: quick-start
toc: true
---

Before you can create an application in Codefresh, you need to create the resource specifications used by the application:

1. Rollout specification
1. Service specification
1. Analysis template specification


Let's start by creating the rollout specification for our application. 


### Create path in Git for application resources
All the resource specifications must be saved in a Git repo. 

* In your Git repo, create a folder to store the resources needed to deploy the application.
  For example, `<runtime-installation-directory>/quick-start/`

### Create a Rollout specification

Create a rollout specification for the application you want to deploy.  
To leverage Argo Rollout's deployment capabilities, we are using the Rollout resource instead of the native Kubernetes Deployment object.
Read about the fields you can define in [Argo Rollout specification](https://argoproj.github.io/argo-rollouts/features/specification/){:target="\_blank"}. 

Our rollout specification uses an analysis template that works with Prometheus a third-party metric provider. You need to first add secrets to the cluster to store the credentials required. 

* In the Git repository create the `rollout.yaml` file as in the example below.


```yaml
apiVersion: argoproj.io/v1alpha1
kind: Rollout
metadata:
  name: codefresh-guestbook-rollout
spec:
  replicas: 4
  revisionHistoryLimit: 2
  selector:
    matchLabels:
      app: codefresh-guestbook
  template:
    metadata:
      labels:
        app: codefresh-guestbook
    spec:
      containers:
        - image: gcr.io/heptio-images/ks-guestbook-demo:0.1
          name: codefresh-guestbook
          ports:
            - name: http
              containerPort: 8080
              protocol: TCP
  minReadySeconds: 30
  strategy:
    canary:
      analysis:
        templates:
          - templateName: background-analysis
        startingStep: 1
      steps:
        - setWeight: 25
        - pause: {duration: 20s}
        - setWeight: 75
        - pause: {duration: 15s}
```

####  Fields in `rollout.yaml`

{: .table .table-bordered .table-hover}
|  Field                             | Notes        |  
| --------------                     | -------------|  
| `replicas`                         | When deployed, the Rollout creates four replicas of the `codefresh-guestbook` application.|  
| `revisionHistoryLimit`             | The number of replica sets to retain.  |      
| `matchLabels`                      | The pods to select for this rollout. In our example, all pods with the label `codefresh-guestbook` are selected.|      
| `image`                            | The container image for the application with the version tag, `gcr.io/heptio-images/ks-guestbook-demo:0.1` in our example.|                             
| `name`                             | The name of the application, `codefresh-guestbook` in our example. |       
| `canary`                           | The deployment strategy, `canary` meaning that the traffic is gradually routed to the new application. Starting with `setWeight` of`25%` followed by a `pause` of 20 seconds, and the remaining `75%` after verification.|  
| `templateName`                      | The analysis template used to validate the application metrics. In our example, uses the template `background-analysis` with Prometheus to monitor and validate metric thresholds.|  


### Create a service specification
* Create a `Service` specification for the application you want to deploy, using the example below.  
  Create it in the same folder in which you saved the `rollouts.yaml` specification. 

```yaml
apiVersion: v1
kind: Service
metadata:
  name: codefresh-guestbook-svc
spec:
  ports:
    - port: 8080
      targetPort: 3000
  selector:
    app: codefresh-guestbook # must be the same as the selector defined in rollouts.yaml
  type: LoadBalancer
```

####  Fields in `service.yaml`

{: .table .table-bordered .table-hover}
|  Service field            |  Notes |  
| --------------            | --------------           |  
| `spec.ports`              | The internal `port`, 8080 in our example, and external `targetPort`, 3000 in our example.| 
| `selector.app`            | The pods to select, and MUST be identical to that defined in `rollouts.yaml`, `codefresh.guestbook` in our example.| 

### Create an AnalysisTemplate for rollout validation
Create an `AnalysisTemplate` specification to validate that your changes conforms to the requirements before deployment. This is the final specification you need before you can create the application.

For the quick start, you'll create the `background-analysis` analysis template, that interfaces with Prometheus, as the third-party metric provider to validate metrics. 
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

####  Fields in `backround-analysis.yaml`

{: .table .table-bordered .table-hover}
|  Service field            |  Notes |  
| --------------            | --------------           |  
| `count`                   | The total number of measurements taken, `4` in our example.| 
| `interval`                | The interval between measurement samplings, `5s` in our example.| 
| `successCondition`        | The requirement for the rollout to be considered a success. In our example, the resulting metric value must be greater than 100.|
| `failureLimit`            | The maximum number of failures permitted, `1` in our example. If the metric value is below 100 more than once, the rollout is aborted.|
| `query`                   | The query submitted to the Prometheus server.|

You are now ready to create the application in Codefresh. 
