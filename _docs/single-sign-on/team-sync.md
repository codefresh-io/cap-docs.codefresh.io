---
title: Common configuration for SSO providers
description: "Team sync, default SSO provider for accounts"
group: single-sign-on
redirect_from:
  - /docs/single-sign-on/team-sync/
toc: true
---

Once you create an SSO provider account in Codefresh, you can:
* Automatically or manually sync between the teams created in Codefresh and your Identity Provider (IdP)
* Set a default SSO provider for your account
* Override the account-level SSO provider for specific users


## Syncing teams with IdPs
Team sync synchronizes all users of the team with the IdP. 

You can sync teams:
* Automatically, in the Codefresh UI when you set up the SSO account for the IdP, through the **Auto-sync team** option. For details, see the SSO setup for your IdP.
* Manually, through the Codefresh CLI's [synchronize teams command](https://codefresh-io.github.io/cli/teams/synchronize-teams/){:target="\_blank"}. 

> Team-sync is supported for OIDC providers.  
  For SAML, team-sync is supported only for Google.

## CLI synchronize teams

As an example, you can sync your Azure teams with the CLI: 

```shell
codefresh synchronize teams <my-client-name> -t azure
```
where:  
`<my-client-name>` is the Client Name/Assertion URL/Callback URL that is automatically generated by Codefresh when you save the SSO configuration for your provider.


{% include image.html
lightbox="true"
file="/images/administration/sso/azure/client-name.png"
url="/images/administration/sso/azure/client-name.png"
alt="SSO Client Name"
caption="SSO Client Name"
max-width="40%"
%}


Though you can run this command manually it makes more sense to run it periodically as a job. And the obvious
way to perform this is with a Codefresh pipeline. The CLI can be used as a [freestyle step]({{site.baseurl}}/docs/pipelines/steps/freestyle/).

You can create a Git repository with a [codefresh.yml]({{site.baseurl}}/docs/pipelines/what-is-the-codefresh-yaml/) file with the following content:

```yaml
version: '1.0'
steps:
  syncMyTeams:
    title: syncTeams
    image: codefresh/cli
    commands:
      - 'codefresh synchronize teams my-client-name -t azure'
```

To fully automate this pipeline, you should set a [cron trigger]({{site.baseurl}}/docs/pipelines/triggers/cron-triggers/) for it. Depending on how you set up your Cron trigger, you can synchronize your teams every day/week/hour. 

### CLI sync and email domain restrictions
If the `Restrict inviting additional users by email address domain` is enabled for your account, running the `synchronize teams` command via the CLI, _does not invite new users_ to Codefresh.  
The output of the command will be similar to the following:

```json
[
  {
    "action": "update",
    "teams": [
      {
        "team": "developers",
        "members": [
          {
            "members": [],
            "action": "create"
          }
        ]
      },
      {
        "team": "DevOps",
        "members": [
          {
            "members": [],
            "action": "create"
          }
        ]
      }
    ]
  }
]
```

**Turn off the domain restriction**:

1. In the Codefresh UI, from your avatar dropdown, click **Account Settings**.
1. In the sidebar, from Access & Collaboration, select **User & Teams**, and then click the **Security** tab.
1. Turn off **Restrict inviting additional users by email address domain**.
1. Click **Save**.
1. Rerun the CLI sync command.

### Sync GitHub Organization Teams to Codefresh

As an admin, you may want to sync your GitHub Organization Teams with your Codefresh account. At the same time, you do not want to set up an SSO provider and have the users use any login provider they choose.

The Personal Access Token (PAT) from a user will sync ALL Organizations and ALL Teams to which the user has access. It is recommended to use a "machine" account to access the one organization you need.

1. Create a PAT that has access to read organizations and teams
1. Install and configure the Codefresh CLI

    `codefresh synchronize teams github -t github --tk $GHTOKEN`

1. The sync will invite all users except for those that have private email settings turned on.

Once the initial sync happens, you can set up a cron trigger pipeline to run the command on a schedule.

## Set a default SSO provider for account

If you have multiple SSO providers, you can set one of them as the default provider for your account. 
Setting a default provider assigns the selected SSO automatically to all new users in the account. The link in the email invitation takes them directly to the login page of that SSO provider.

1. In the Codefresh UI, go to [Single Sign-On](https://g.codefresh.io/2.0/account-settings/single-sign-on).
1. From the list, select the SSO account to set as default and click the **Edit** icon on the right.
1. Scroll down and select **Set as default**. 
<!---change screenshot
{% include image.html
lightbox="true"
file="/images/administration/sso/default-sso.png"
url="/images/administration/sso/default-sso.png"
alt="Default SSO provider"
caption="Default SSO provider"
max-width="90%"
%} -->


## Select SSO provider for individual users

You can override the default SSO provider if set for your account, with a different SSO provider for specific users if so required.  
* New users   
  If you have an SSO provider selected as the default, that provider is automatically assigned to new users, added either manually or via team synchronization.  

* Existing users  
  SSO login is not configured by default for existing users. You must _explicitly select_ the SSO provider for existing users.  
  If SSO login is already configured for an existing user, and you add a new identity provider, to change the SSO login to the new provider, you must _select_ the new provider for the user. 

1. In the Codefresh UI, on the toolbar, from your avatar dropdown, select **Account Settings**.
1. In the sidebar, from Access & Collaboration, select [**Users & Teams**](https://g.codefresh.io/account-admin/collaborators/users){:target="\_blank"}.   
1. For the user, select the SSO provider from the SSO list.

{% include image.html
lightbox="true"
file="/images/administration/sso/select-user-sso.png"
url="/images/administration/sso/select-user-sso.png"
alt="Selecting SSO method"
caption="Selecting SSO method"
max-width="50%"
%}

## Related articles
[Setting up OIDC Federated SSO]({{site.baseurl}}/docs/single-sign-on/oidc)  
[Setting up SAML2 Federated SSO]({{site.baseurl}}/docs/single-sign-on/saml)  


