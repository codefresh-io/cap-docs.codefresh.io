---
title: "Git tokens"
description: ""
group: reference
redirect_from:
  - /docs/administration/git-tokens/ 
toc: true
---



Codefresh requires two types of Git tokens for authentication:
* A token for  runtime installation (Git runtime token)
* A personal access token for each runtime, unique to every user (Git user token)

The Git runtime token is used for runtime installation. After installation, you need to authorize Git access for every provisioned runtime either through OAuth2 or through a personal access token from your Git provider.  
Every user can see the list of runtimes and the personal access tokens assigned to each runtime in [User Settings](https://g.codefresh.io/2.0/user-settings){:target="\_blank"}. Codefresh always flags and notifies you of invalid, revoked, or expired tokens.


### Git runtime tokens
The Git runtime token is required to provision Codefresh runtimes. The Git runtime token is specific to a runtime, and is mandatory for runtime installation. 


#### Git runtime token permissions
Git runtime tokens need both `repo` and `admim repo` access to create webhooks for Git events.

{% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/quick-start/quick-start-git-event-permissions.png" 
   url="/images/getting-started/quick-start/quick-start-git-event-permissions.png" 
   alt="Permissions for Git runtime token" 
   caption="Permissions for Git runtime token"
   max-width="60%" 
   %}


### Git personal tokens
The Git personal token is a user-specific personal access token per provisioned runtime. Unique to each user, it is required to authenticate Git-based actions per runtime in Codefresh after installation. 
You can manage personal access tokens for runtimes using either OAuth to authorize access or a personal access token from your Git provider.

> If you have access to multiple runtimes, you can use the same personal access token for all the runtimes.   
  You must configure the token for each runtime.

#### Git personal token permissions
Git personal tokens need `repo` access for commits and other actions.

{% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/github-pat.png" 
   url="/images/getting-started/github-pat.png" 
   alt="Permissions for Git personal token" 
   caption="Permissions for Git personal token"
   max-width="60%" 
   %}

### Related articles  
[User settings]({{site.baseurl}}/docs/administration/user-settings/)