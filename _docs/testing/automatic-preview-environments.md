---
title: "Dynamic preview environments"
excerpt: ""
description: ""
excerpt: ""
group: testing
redirect_from:
  - /docs/launch-composition/
  - /docs/on-demand-test-environment/launch-composition-at-the-end-of-the-build/
toc: true
old_url: /docs/launch-composition
---
If your service is one of many microservices, after running automated tests on your service, you would probably want to check the new service with your whole system. In this case, you can launch the composition of your system as part of your build, and at the end of the build, open the composition.

## Prerequisites

Complete the tutorials for:  
* [Creating a basic pipeline]({{site.baseurl}}/docs/getting-started/create-a-basic-pipeline/)
* [Creating temporary environments]({{site.baseurl}}/docs/getting-started/on-demand-environments/).

## Launch the composition

{:start="1"}
1. Open your `codefresh.yml` file and add a new step:

  `YAML`
{% highlight yaml %}
{% raw %}
launch_composition_step:
    title: "Launch full composition with latest images"
    type: launch-composition
    composition: your-composition-name
    fail_fast: false
{% endraw %}
{% endhighlight %}


1. Commit and push the changes to Git repository.
1. Build your service with Codefresh.
1. From the Artifacts section on the sidebar, select **Compositions**, and then select the **Running Compositions** tab.  
   The new preview environment is displayed in the list of Running Compositions.

## Launch an environment only on one branch

There is a limit to the number of environments you can run concurrently. That's why it's a good practice to launch the composition only on a certain condition. Usually the most relevant condition is the branch, since you probably want your environment to be updated by on the main branch.

The following instructions describes how to launch the environment for only the `master` branch: 

{:start="1"}
1. Open your `codefresh.yml` file and add to the `launch_composition_step` the following:

  `YAML`
{% highlight yaml %}
{% raw %}
when:
    branch:
      only:
        - master
{% endraw %}
{% endhighlight %}

1. Commit and push changes to your Git repository's `master` branch.
1. Build your service with Codefresh on branch `master`.
1. Create a new branch and push it to your Git repository under a new branch.
1. Build your service with Codefresh on the new branch.
1. When the build completes execution, open its log.  
   You should see something similar to the example below.

{% include image.html 
lightbox="true" 
file="/images/df14346-Screen_Shot_2017-01-19_at_10.29.16.png" 
url="/images/df14346-Screen_Shot_2017-01-19_at_10.29.16.png"
alt="Screen Shot 2017-01-19 at 10.29.16.png"
max-width="40%" 
%}

## Related articles
[Codefresh yaml]({{site.baseurl}}/docs/pipelines/what-is-the-codefresh-yaml/)  
[Creating compositions]({{site.baseurl}}/docs/testing/create-composition/)  
[Integration tests]({{site.baseurl}}/docs/testing/integration-tests/)  
[Service containers]({{site.baseurl}}/docs/pipelines/service-containers/)
