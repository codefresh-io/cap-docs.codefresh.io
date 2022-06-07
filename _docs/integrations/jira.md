---
title: "JIRA tracking"
description: "Know which features are deployed on which environment"
group: integrations
toc: true
---

One of the major highlights of Codefresh is the ability to correlate automatically
software features and where/when they are deployed. It is rather easy to know the software version of a component, but in most cases people are not really interested in numerical versions, but instead which features are contained in a release.

Codefresh has native integration for Atlassian Jira. This way Codefresh
monitors a feature all the way from the ticket creation phase, up to the moment it is implemented and deployed to an environment.

## Required JIRA information

First you need to obtain your JIRA instance credentials. Follow the [Attlassian documentation](https://support.atlassian.com/atlassian-account/docs/manage-api-tokens-for-your-atlassian-account/) on how to get them.

You should note down:

* Your JIRA URL
* The JIRA username/email that will be used for the integration
* The JIRA password/token that was created for this user

Once you have these values, you can enter them on the Codefresh side.

## Codefresh configuration

Navigate to the integration page by going to [https://g.codefresh.io/2.0/account-settings/integrations](https://g.codefresh.io/2.0/account-settings/integrations). 

Choose *Atlassian JIRA* from the cards and click on the *Configure*.
On the next screen click the *+Add* button on the top right. 

1. Enter a name for your integration (you can have multiple JIRA instances connected)
2. Choose the scope of the integration in the *enable for* section
3. Choose whether you want to use this integration on all Codefresh runtimes or a specific runtime.

Enter the JIRA details from the previous step

* JIRA host - URL of your JIRA instance
* API Token - password/token
* API email - user email for the token

## What to read next

* [Adding Git sources]({{site.baseurl}}/docs/runtime/git-sources/)










