---
title: "Get Short SHA ID and Use it in a CI Process"
description: ""
group: example-catalog
sub_group: ci-examples
redirect_from:
  - /docs/how-to-guides/
  - /docs/how-get-first-8-digits-of-sha/
toc: true
old_url: /docs/how-get-first-8-digits-of-sha
---

## Get the short SHA ID
Put the following variable in your script

{{site.data.callout.callout_info}}
{% highlight text %}
{% raw %}
${{CF_SHORT_REVISION}} 
{% endraw %}
{% endhighlight %}
{{site.data.callout.end}}

## Use the SHA ID in a tag

{{site.data.callout.callout_info}}
{% highlight text %}
{% raw %}
tag: ${{CF_SHORT_REVISION}} 
{% endraw %}
{% endhighlight %}
{{site.data.callout.end}}

## YAML example

{% highlight yaml %}
{% raw %}
step-name:
  type: build
  description: Free text description
  working-directory: ${{clone-step-name}}
  dockerfile: path/to/Dockerfile
  image-name: owner/new-image-name
  tag: ${{CF_SHORT_REVISION}}
  build-arguments:
    - key=value
  fail-fast: false 
{% endraw %}
{% endhighlight %}

## Result in [hub.docker](https://hub.docker.com){:target="_blank"}

{% include image.html 
lightbox="true" 
file="/images/b694c19-2016-10-18_17-42-38.png" 
url="/images/b694c19-2016-10-18_17-42-38.png"
alt="2016-10-18_17-42-38.png"
max-width="40%"
%}

## Result in Codefresh

{% include image.html 
lightbox="true" 
file="/images/de17013-2016-10-18_17-49-22.png" 
url="/images/de17013-2016-10-18_17-49-22.png"
alt="2016-10-18_17-49-22.png"
max-width="40%"
%}


## Related articles 
[Codefresh YAML]({{site.baseurl}}/docs/pipelines/what-is-the-codefresh-yaml/)    
[Creating pipelines]({{site.baseurl}}/docs/pipelines/pipelines/)  
[How Codefresh pipelines work]({{site.baseurl}}/docs/pipelines/introduction-to-codefresh-pipelines/)  