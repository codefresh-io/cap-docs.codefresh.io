---
title: "Git token management"
description: ""
group: administration
toc: true
---



CSDP requires two types of Git tokens, a Git runtime token per runtime, and a Git personal access token, for each runtime, unique to every user. If you have access to multiple runtimes as a user in CSDP, you require a personal access token for each runtime. 

Users can update expired, revoked, or invalid Git runtime and personal user tokens. 

### Git runtime tokens
The Git runtime token is a dedicated token required to provision CSDP runtimes. The Git runtime token is specific to a runtime, and is mandatory for runtime installation. 

An expired, revoked, or invalid Git runtime token, is flagged by a notification in the CSDP UI. You can then generate a new Git runtime token from your Git provider, and update it in CSDP. 

#### Git runtime token permissions
Git runtime tokens need both repo and admim repo access to create webhooks for Git events.

{% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/quick-start/quick-start-git-event-permissions.png" 
   url="/images/getting-started/quick-start/quick-start-git-event-permissions.png" 
   alt="Permissions for Git runtime token" 
   caption="Permissions for Git runtime token"
   max-width="30%" 
   %}

#### How to update a Git runtime token
Update Git runtime tokens as needed. 

**Before you begin**  
* Generate a new runtime token with the correct permissions  

**How to**  

1. In the CSDP UI, when you see a notification, select **[Update Token]**.
  In the **Runtimes** page, runtimes with invalid tokens are prefixed by the key icon. Mouse over shows invalid token.
1. Select the runtime, and then on the top-right of the page, select  and then **+Add Token**. 
1. Paste the generated personal access token. 
1. If there are no validation errors, select **Add**.

### Git personal tokens
The Git personal token is a user-specific personal access token per provisioned runtime. Unique to each user, it is required to authenticate Git-based actions per runtime in CSDP.  

#### Git personal token permissions
Git personal tokens need repo access for commits and other actions.

{% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/quick-start/github-pat.png" 
   url="/images/getting-started/quick-start/github-pat.png" 
   alt="Permissions for Git personal token" 
   caption="Permissions for Git personal token"
   max-width="30%" 
   %}

#### How to update a Git personal token
Update Git personal access tokens for each runtime when needed.

**Before you begin**  
* Generate a new personal access token with the correct permissions  

**How to**  

1. In the CSDP UI, go to [User Settings](https://g.codefresh.io/2.0/user-settings){:target="\_blank"}.
1. Select **+Add Token**. 
1. Paste the generated personal access token. 
1. If there are no validation errors, select **Add**.
