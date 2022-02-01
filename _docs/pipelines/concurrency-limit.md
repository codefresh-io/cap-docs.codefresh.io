---
title: "Selectors for concurrency synchronization"
description: ""
group: pipelines
toc: true
---


Argo Workflows has a synchronization mechanism to limit parallel execution of specific workflows, or templates within workflows, as required.  
The mechanism enforces this with either semaphore or mutex synchronization configurations. Semaphore synchronization is configured in the `ConfigMap`, which is referenced from workflows or templates within workflows.

CSDP supports an additional level of concurrency synchronization for both workflows and templates, with selectors. 
Selectors enable access to dynamic values such as annotations/labels/parameters with templating, providing more flexible concurrency configurations for parallel executions of workflows or templates within workflows. nd finally, selectors support semaphoe and mutex 
and  that give us an ability to . The template will be resolved and used as a selector value.

Concuurency selector definitions
A concurrency synchronization selector is defined through a  `name` `template` pair. 
The name is any meaningful/logical name that describes the selector. 
The template is resolved to the selector value. 

With selectors, you can introduce modularity and define different templating values

Where do you add them?
To the workflow template synchromziation under selectors

Default concurrency synchronization

Scenario: Two workflow templates that share the same semaphore configuration, and key with weight 1. Only one workflow at a time can use the key. Meaning that when workflow 1 is Running, workflow 2 is in Pending status. 

Workflows with default concurrency configuration

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Workflow
metadata:
 generateName: synchronization-wf-1
 labels:
   argo.sensor: github
   argo.sensor-event: push
spec:
 entrypoint: whalesay
 synchronization:
   semaphore:
     configMapKeyRef:
       name: my-config
       key: workflow
templates:
   - name: whalesay
     container:
       image: docker/whalesay:latest
       command: [sleep]
       args: ["5000"]
```

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Workflow
metadata:
 generateName: synchronization-wf-2
 labels:
   argo.sensor: github
   argo.sensor-event: push
spec:
 entrypoint: whalesay
 synchronization:
   semaphore:
     configMapKeyRef:
       name: my-config
        key: workflow
templates:
   - name: whalesay
     container:
       image: docker/whalesay:latest
       command: [sleep]
       args: ["5000"]
```


Workflows with selector currency configuration

<table>
<tr>
<th>workflow-synchronization-1</th>
<th>workflow-synchronization-2</th>
</tr>
<tr>
<td>
<pre>
 ```yaml
apiVersion: argoproj.io/v1alpha1
kind: Workflow
metadata:
 generateName: synchronization-wf-1
 labels:
   argo.sensor: github
   argo.sensor-event: push
spec:
 entrypoint: whalesay
 arguments:
   parameters:
   - name: REPO_OWNER
     value: denis-codefresh
   - name: REPO_NAME
     value: argo-workflows
   - name: GIT_BRANCH
     value: main
 synchronization:
   semaphore:
     configMapKeyRef:
       name: my-config
       key: workflow
     selectors:
       - name: repository
         template: "{{ workflow.parameters.REPO_OWNER }}/{{ workflow.parameters.REPO_NAME }}"
       - name: branch
         template: "{{ workflow.parameters.GIT_BRANCH }}"
 
 templates:
   - name: whalesay
     container:
       image: docker/whalesay:latest
       command: [sleep]
       args: ["5000"]
  ```
</pre>
</td>
<td>
<pre>
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Workflow
metadata:
 generateName: synchronization-wf-2
 labels:
   argo.sensor: github
   argo.sensor-event: push
spec:
 entrypoint: whalesay
 arguments:
   parameters:
   - name: REPO_OWNER
     value: denis-codefresh
   - name: REPO_NAME
     value: argo-workflows
   - name: GIT_BRANCH
     value: main
 synchronization:
   semaphore:
     configMapKeyRef:
       name: my-config
       key: workflow
     selectors:
       - name: repository
         template: "{{ workflow.parameters.REPO_OWNER }}/{{ workflow.parameters.REPO_NAME }}"
       - name: branch
         template: "{{ workflow.parameters.GIT_BRANCH }}"
 
 templates:
   - name: whalesay
     container:
       image: docker/whalesay:latest
       command: [sleep]
       args: ["5000"]
  ```
</pre>
<td>
</tr>
</table>





