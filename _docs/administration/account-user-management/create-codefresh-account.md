---
title: "Create a Codefresh account"
description: "Welcome to Codefresh!"
group: getting-started
redirect_from:
  - /docs/
  - /docs/create-an-account/
  - /docs/getting-started/
  - /docs/getting-started/introduction/
---
Before you can do anything in Codefresh such as building and deploying your applications, you need to create a Codefresh account.

Creating an account in Codefresh is free (no credit card is required) and can be done in three simple steps

{% include 
image.html 
lightbox="true" 
file="/images/getting-started/create-account/create-account-steps.png" 
url="/images/getting-started/create-account/create-account-steps.png"
alt="Codefresh account creation steps" 
max-width="90%" 
%}

## Step 1: Select your Identity Provider
As the first step in setting up ypur account in Codefresh, select the identity provider (IdP) to use. 
Codefresh currently supports the following IdPs:
GitHub
Bitbucket
GitLab 
Azure
Google 
LDAP

If you need an IdP that is not in the list, please [contact us](https://codefresh.io/contact-us/) with the details.

>NOTES:  
  For Git repositories, the login method is less important, as you can Git repositories through [Git integrations]({{site.baseurl}}/docs/integrations/git-providers/), regardless of your sign-up process.  

  If you multiple sign-up methods, as long as you use the same email address in all the sign-ups, Codefresh automatically redirects you to the account dashboard.

1. Go to the [Codefresh Sign Up page](https://g.codefresh.io/signup).  <!---need to change the URL and the screenshot-->


{% include 
image.html 
lightbox="true" 
file="/images/getting-started/create-account/select-identity-provider.png" 
url="/images/getting-started/create-account/select-identity-provider.png"
alt="Codefresh sign-up page" 
caption="Sign-up page (click image to enlarge)" 
max-width="40%" 
%}

1. Select the IdP for sign-up.
1. Continue 



## Step 2: Accept the permissions request

After you select the IdP (identity provider), Codefresh requests permission to access your basic details, and for Git providers, to access your Git repositories). The Permissions window that is displayed differs according to the IdP selected in the previous step.

Don't worry, Codefresh will not do anything without your explicit approval, so don't be scared by the permissions shown
in the request window. The permissions requested by Codefresh are needed in order to build and deploy your projects.

1. Click 

  * For GitHub: To continue, click **Authorize codefresh-io**.

{% include 
image.html 
lightbox="true" 
file="/images/getting-started/create-account/github-authorize.png" 
url="/images/getting-started/create-account/github-authorize.png"
alt="GitHub authorization page" 
caption="GitHub authorization page (click image to enlarge)" 
max-width="50%" 
%}

* For Bitbucket: To continue, click **Grant access**.


{% include 
image.html 
lightbox="true" 
file="/images/getting-started/create-account/bitbucket-authorize.png" 
url="/images/getting-started/create-account/bitbucket-authorize.png"
alt="Bitbucket authorization page" 
caption="Bitbucket authorization page (click image to enlarge)" 
max-width="50%" 
%}

* For GitLab: To continue, click **Authorize**.


{% include 
image.html 
lightbox="true" 
file="/images/getting-started/create-account/gitlab-authorize.png" 
url="/images/getting-started/create-account/gitlab-authorize.png"
alt="GitLab authorization page" 
caption="GitLab authorization page (click image to enlarge)" 
max-width="50%" 
%}

Once you confirm the permissions for your Git provider, Codefresh automatically connects to your Git provider and fetches your basic account details, such as your email.


## Step 3: Verify account details

Verifying account details is the final step in creating your Codefresh account. 

1. Review the details for your new account, make the relevant changes, and click **NEXT**. 

{% include 
image.html 
lightbox="true" 
file="/images/getting-started/create-account/codefresh-signup.png" 
url="/images/getting-started/create-account/codefresh-signup.png" 
alt="Codefresh account details" 
caption="Codefresh account details (click image to enlarge)" 
max-width="40%" 
%}

{:start="2"}
1. Enter a name for your account, and click **NEXT**.

{% include 
image.html 
lightbox="true" 
file="/images/getting-started/create-account/codefresh-accountname.png" 
url="/images/getting-started/create-account/codefresh-accountname.png" 
alt="Codefresh account name" 
caption="Codefresh account name (click image to enlarge)" 
max-width="40%" 
%}

{:start="3"}
1. Finally, answer the questions to personalize your account and click **FINISH**.

{% include 
image.html 
lightbox="true" 
file="/images/getting-started/create-account/codefresh-personalize.png" 
url="/images/getting-started/create-account/codefresh-personalize.png" 
alt="Codefresh personalize account" 
caption="Codefresh personalize account (click image to enlarge)" 
max-width="40%" 
%}

Congratulations! Your new Codefresh account is now ready.

{% include 
image.html 
lightbox="true" 
file="/images/getting-started/create-account/codefresh-dashboard.png" 
url="/images/getting-started/create-account/codefresh-dashboard.png" 
alt="Codefresh dashboard" 
caption="Codefresh dashboard (click image to enlarge)" 
max-width="40%" 
%}



<!---nned to verify
The next step is learning how to [build your first application]({{site.baseurl}}/docs/getting-started/create-a-basic-pipeline/).


## Other Git connection options



Codefresh also supports Atlassian Stash/Bitbucket Server. You need to [contact us](https://codefresh.io/contact-us/) to enable this integration before you can use it for your account.


{% include 
image.html 
lightbox="true" 
file="/images/getting-started/create-account/stash.png" 
url="/images/getting-started/create-account/stash.png" 
alt="Codefresh and Atlassian Stash" 
caption="Codefresh and Atlassian Stash" 
max-width="100%" 
%}


Once that is done, follow the [Stash instructions]({{site.baseurl}}/docs/integrations/git-providers/#atlassian-stash) for more information. 



## Using Codefresh in a secure corporate environment

If your source code repositories are in a private Git account that lies behind your company firewall, or simply has no access to the Internet, we can still help you!

{% include 
image.html 
lightbox="true" 
file="/images/getting-started/create-account/git-firewall.png" 
url="/images/getting-started/create-account/git-firewall.png" 
alt="Git behind firewall" 
caption="Git behind firewall" 
max-width="100%" 
%}

We can establish a VPN / tunnel to your network or discuss options for an on-premises Codefresh deployment. Please [contact us to get started](https://codefresh.io/contact-us/).

-->
## What to read next

* [Create a Basic pipeline]({{site.baseurl}}/docs/getting-started/create-a-basic-pipeline/)
* [Deploy to Kubernetes]({{site.baseurl}}/docs/getting-started/deployment-to-kubernetes-quick-start-guide/)
* [Introduction to Codefresh pipelines]({{site.baseurl}}/docs/pipelines/introduction-to-codefresh-pipelines/)
* [Pipeline examples]({{site.baseurl}}/docs/yaml-examples/examples/)


