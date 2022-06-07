---
title: "Quay Registry"
description: "Push, Pull and Deploy images from Quay"
group: integrations
toc: true
---

Codefresh has native support for interacting with Quay registries.

## Required Quay information

[Create a Redhat/Quay account at Quay](https://quay.io/) and then optionally  [create a robot account](https://docs.quay.io/glossary/robot-accounts.html) for Codefresh integration.

## Codefresh configuration

Navigate to the integration page by going to [https://g.codefresh.io/2.0/account-settings/integrations](https://g.codefresh.io/2.0/account-settings/integrations). 

Choose *Quay* from the cards and click on the *Add*.
On the next screen click the *+Add* button on the top right. 

1. Enter a name for your integration (you can have multiple Quay instances connected)
3. Choose whether you want to use this integration on all Codefresh runtimes or a specific runtime.

Then fill in

* Domain - `quay.io`
* Username - your Quay.io username.
* Password - your Quay.io encrypted password (or robot account)

## What to read next

* [Adding Git sources]({{site.baseurl}}/docs/runtime/git-sources/)










