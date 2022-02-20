---
title: "Selectors for concurrency synchronization"
description: ""
group: pipelines
toc: true
---


Argo Workflows has a synchronization mechanism to limit parallel execution of specific workflows, or templates within workflows, as required.  
The mechanism enforces this with either semaphore or mutex synchronization configurations.  

CSDP supports an additional level of concurrency synchronization with selectors for both workflows and templates. 
Selectors enable access to dynamic values with k8s objects such as `annotations`, `labels`, or `parameters`, with templating, providing more flexible concurrency configurations for parallel executions of workflows or templates within workflows. And finally, selectors work with both semaphore and mutex synchronization configurations.  

Semaphore synchronization is configured in the `ConfigMap`, which is referenced from workflows or templates within workflows.  

Here's an example of the `ConfigMap` with the concurrency definition:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
 name: my-config
data:
 workflow: "1"
```

The examples in this article are for semaphore synchronization configurations.

### Concurrency selector definitions
A concurrency synchronization selector is defined through a `name`-`template` pair in the Workflow Template:  

* The `name` is any meaningful/logical name that describes the selector. For example, Git repo or branch.
* The `template` is the parameter mapping for the name, that is resolved to the selector value when the pipeline is triggered. For example, `{{ workflow.parameters.REPO_OWNER }}/{{ workflow.parameters.REPO_NAME }}` or `{{ workflow.parameters.GIT_BRANCH }}`.  

Where do you add them?  

To the `synchromziation` section in the Workflow Template, under `selectors`, as in the sample YAML below.

{% highlight yaml %}
{% raw %}
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
{% endraw %}
{% endhighlight %}

### When and how does selector concurrency configuration work?

Let's review different scenarios to illustrate when and how selector-based concurrency synchronization works.  

We have two workflows, `synchronization-wf-1` and `synchronization-wf-2`. 

#### Default concurrency synchronization
The have the same default values for the arguments, and share the same semaphore configuration, and key with weight 1.   

{% include image.html 
  lightbox="true" 
  file="/images/pipeline/concurrency/default-concurrency-sync.png" 
  url="/images/pipeline/concurrency/default-concurrency-sync.png"
       alt="Workflows with default concurrency configuration"
       caption="Workflows with default concurrency configuration"
       max-width="30%"
       %}

With the default concurrency configuration, only one workflow at a time can use the key. Meaning that while `synchronization-wf-1` is `Running`, `synchronization-wf-2` is in `Pending` status.

#### Selector concurrency synchronization with identical values 

Here are the same workflows, `synchronization-wf-1` and `synchronization-wf-2`, this time configured with the _selector_ concurrency.  
Both workflows share the same semaphore key with weight of 1.  
Both workflows also _use the same set of selectors_. The Git repo and the Git branch arguments have the same values in both.  

{% include image.html 
  lightbox="true" 
  file="/images/pipeline/concurrency/selector-concurrency-sync.png" 
  url="/images/pipeline/concurrency/selector-concurrency-sync.png"
       alt="Workflows with selector concurrency configuration"
       caption="Workflows with selector concurrency configuration"
       max-width="30%"
       %}

The selectors have no impact in this case, as both workflows use the same set of selectors.
If you look at the workflow status, you can see the key:

```yaml
synchronization:
 semaphore:
   holding:
   - holders:
     - synchronization-wf-6lf8b
     semaphore: argo/ConfigMap/my-config/workflow?repository=denis-codefresh/argo-workflows&branch=main
```


#### Selector concurrency synchronization with value differentiation

In the third scenario, we have the same workflows, `synchronization-wf-1` and `synchronization-wf-2`, also configured with the _selector_ concurrency.  
They also share the same semaphore configuration and key with weight 1.  

The fundamental difference is that in this scenario, they use _different_ selectors. In `synchronization-wf-1`, the Git branch is set to `main`, and in `synchronization-wf-2`, to `feature`. 

{% include image.html 
  lightbox="true" 
  file="/images/pipeline/concurrency/selector-concurrency-sync-git-branch.png" 
  url="/images/pipeline/concurrency/selector-concurrency-sync-git-branch.png"
       alt="Workflows with selector concurrency and different Git branches"
       caption="Workflows with selector concurrency and different Git branches"
       max-width="30%"
       %}

Now, because the workflows use _different selectors_, both workflows run in parallel.
