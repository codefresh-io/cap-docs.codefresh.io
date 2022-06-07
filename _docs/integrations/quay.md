---
title: "Quay Registry"
description: ""
group: integrations
toc: true
---

Codefresh has native support for interacting with Quay registries, from where you can push, pull, and deploy images.

## Prerequisites


1. [Create a Redhat/Quay account at Quay](https://quay.io/)
1. Optional. For Codefresh integration,[Create a robot account](https://docs.quay.io/glossary/robot-accounts.html) 

## Configure Quay integration in Codefresh

1. In the Codefresh UI, go to [Integrations](https://g.codefresh.io/2.0/account-settings/integrations){:target="\_blank"}. 
1. On the **Quay Docker Registry** card, click **Add**.
1. Click **Add** on the top-right. 
1. Configure the Quay integration settings:
  * Enter an **Integration name**. You can have multiple Quay instances connected.
  * Use this integration for **All runtimes**, or specific **Selected runtimes**.
  * Set the **Domain** to `quay.io`.
  * **Username**: The Quay.io username.
  * **Password**: The Quay.io encrypted password, or robot account if you created one.

 {% include image.html 
 lightbox="true" 
 file="/images/integrations/quay/quay-int-settings.png" 
  url="/images/integrations/quay/quay-int-settings.png"
  alt="Quay Docker Registry integration settings in Codefresh"
  caption="Quay Docker Registry integration settings in Codefresh"
  max-width="50%"
  %}

## What to read next  

[Adding Git sources]({{site.baseurl}}/docs/runtime/git-sources/)
