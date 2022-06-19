---
title: "Create an Argo Rollout specification"
description: ""
group: getting-started
sub-group: quick-start
toc: true
---


### Create path in Git for application resources
In your Git repo, create a folder to store the resources needed to deploy the application.
For example, `<runtime-installation-directory>/quick-start/`


### Create a Rollout specification

Create a rollout specification for the application you want to deploy.  
To leverage Argo Rollout's deployment capabilities, we are using the Rollout resource instead of the native Kubernetes Deployment object.
Read about the fields you can define in [Argo Rollout specification](https://argoproj.github.io/argo-rollouts/features/specification/){:target="\_blank"}. 

Our rollout specification uses an analysis template that works with Prometheus a third-party metric provider. You need to first add secrets to the cluster to store the credentials required. 



1. In the Git repository create the `rollout.yaml` file as in the example below.


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
Notes on rollout fields
`replicas`: When deployed, the Rollout creates four replicas of the `codefresh-guestbook` application.
`revisionHistoryLimit` is the number of replica sets to retain. 
`matchLabels` defines the pods to select for this rollout. In our example, all pods with the label `codefresh-guestbook` are selected.
`image` and `name` define the container image for the application, `gcr.io/heptio-images/ks-guestbook-demo:0.1` in our example, and the name of the application, `codefresh-guestbook` in our example.

`strategy` section defines the deployment strategy and how it is implemented: 
For the codefresh-guestbook appplication, we use the following rollout strategy:
`canary` strategy with gradual traffic routing, at  `20%` of the traffic to the new version, followed by a pause of 20 seconds. After analysis verification, routes the remaining 75% of user traffic to the new application.
Uses analyis template: background analyis against Prometheus to verify metric thresholds  
 of routes  that traffic is routed using a weightage of 25 and then 75 with a pause duration of 20 seconds