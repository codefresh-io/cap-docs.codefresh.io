---
title: "Create a Service manifest"
description: ""
group: getting-started
sub-group: quick-start
toc: true
---

Create a Kubernetes Service specification using YAML, one of two manifest specifications to create an application in Codefresh.  
The Service spec targets the pods on which your application is deployed, and exposes them outside of your cluster to receive traffic.



### Create a service specification
Create a `Service` specification for the application you want to deploy, using the example below.  
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


{: .table .table-bordered .table-hover}
|  Service field            |  Notes |  
| --------------            | --------------           |  
| `spec.ports`              | The internal `port`, 8080 in our example, and external `targetPort`, 3000 in our example.| 
| `selector.app`            | The pods to select, and MUST be identical to that defined in `rollouts.yaml`, `codefresh.guestbook` in our example.| 
  
 
