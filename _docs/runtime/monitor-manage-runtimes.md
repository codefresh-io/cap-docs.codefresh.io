---
title: "Manage provisioned runtimes"
description: ""
canonical_url: 'https://codefresh.io/docs/docs/installation/gitops/monitor-manage-runtimes/'
group: runtime
redirect_from:
  - /monitor-manage-runtimes/
  - /monitor-manage-runtimes  
toc: true
---


The **Runtimes** page displays the provisioned runtimes in your account, both hybrid, and the hosted runtime if you have one.  

View runtime components and information in List or Topology view formats, and upgrade, uninstall, and migrate runtimes.  

{% include
   image.html
   lightbox="true"
   file="/images/runtime/runtime-list-view.png"
 url="/images/runtime/runtime-list-view.png"
  alt="Runtime List View"
  caption="Runtime List View"
  max-width="70%"
%}

Select the view mode to view runtime components and information, and manage provisioned runtimes in the view mode that suits you.  


Manage provisioned runtimes: 
* [Update Git tokens for runtimes](#update-git-tokens-for-runtimes)
* [Configure SSH for runtimes](#configure-ssh-for-runtimes)
* [Add managed clusters to hybrid or hosted runtimes]({{site.baseurl}}/docs/runtime/managed-cluster/)
* [Add and manage Git Sources associated with hybrid or hosted runtimes]({{site.baseurl}}/docs/runtime/git-sources/)
* [Reset shared configuration repository](#reset-shared-configuration-repository)
* [Upgrade provisioned hybrid runtimes](#hybrid-upgrade-provisioned-runtimes)
* [Upgrade Codefresh CLI](#upgrade-codefresh-cli)
* [Uninstall provisioned runtimes](#uninstall-provisioned-runtimes)
* [Update Git tokens for runtimes](#update-git-tokens-for-runtimes)
<!--* [Migrate access mode for hybrid runtimes](#hybrid-migrate-runtimes) -->

> Unless specified otherwise, management options are common to both hybrid and hosted runtimes. If an option is valid only for hybrid runtimes, it is indicated as such. 

To monitor provisioned hybrid runtimes, including recovering runtimes for failed clusters, see [Monitor provisioned hybrid runtimes]({{site.baseurl}}/docs/runtime/monitoring-troubleshooting/).

### Runtime views

View provisioned hybrid and hosted runtimes in List or Topology view formats.  

* List view: The default view, displays the list of provisioned runtimes, the clusters managed by them, and Git Sources.
* Topology view: Displays a hierarchical view of runtimes and the clusters managed by them, with health and sync status of each cluster.

#### List view

The List view is a grid-view of the provisioned runtimes.  

Here is an example of the List view for runtimes.
{% include
   image.html
   lightbox="true"
   file="/images/runtime/runtime-list-view.png"
 url="/images/runtime/runtime-list-view.png"
  alt="Runtime List View"
  caption="Runtime List View"
  max-width="70%"
%}

Here is a description of the information in the List View.

{: .table .table-bordered .table-hover}
| List View Item|  Description   |
| --------------          | ---------------- |
|**Name**| The name of the provisioned Codefresh runtime.  |
|**Type**| The type of runtime provisioned, and can be **Hybrid** or **Hosted**.  |
|**Cluster/Namespace**| The K8s API server endpoint, as well as the namespace with the cluster. |
|**Modules**| The modules installed based on the type of provisioned runtime. Hybrid runtimes include CI and CD Ops modules. Hosted runtimes include CD Ops.   |
|**Managed Cluster**| The number of managed clusters if any, for the runtime. To view list of managed clusters, select the runtime, and then the **Managed Clusters** tab.  To work with managed clusters, see [Adding external clusters to runtimes]({{site.baseurl}}/docs/runtime/managed-cluster).|
|**Version**| The version of the runtime currently installed. **Update Available!** indicates there are later versions of the runtime. To see all the commits to the runtime, mouse over **Update Available!**, and select **View Complete Change Log**.
|**Last Updated**| The most recent update information from the runtime to the Codefresh platform. Updates are sent to the platform typically every few minutes. Longer update intervals may indicate networking issues.|
|**Sync Status**| The health and sync status of the runtime or cluster.  {::nomarkdown}<br><ul><li> <img src="../../../images/icons/error.png"  display=inline-block> indicates health or sync errors in the runtime, or a managed cluster if one was added to the runtime.</br> The runtime name is colored red.</li> <li><img src="../../../images/icons/cf-sync-status.png"  display=inline-block> indicates that the runtime is being synced to the cluster on which it is provisioned.</li></ul> {:/} |

#### Topology view

A hierarchical visualization of the provisioned runtimes. The Topology view makes it easy to identify key information such as version, health and sync status, for both the provisioned runtime and the clusters managed by it.  
Here is an example of the Topology view for runtimes.
  {% include
 image.html
 lightbox="true"
 file="/images/runtime/runtime-topology-view.png"
 url="/images/runtime/runtime-topology-view.png"
 alt="Runtime Topology View"
 caption="Runtime Topology View"
  max-width="30%"
%}

Here is a description of the information in the Topology view.

{: .table .table-bordered .table-hover}
| Topology View Item      | Description   |
| ------------------------| ---------------- |
|**Runtime**             | ![](../../../images/icons/codefresh-runtime.png?display=inline-block) the provisioned runtime. Hybrid runtimes display the name of the K8s API server endpoint with the cluster. Hosted runtimes display 'hosted'.  |
|**Cluster**              | The local, and managed clusters if any, for the runtime. {::nomarkdown}<ul><li><img src="../../../images/icons/local-cluster.png" display=inline-block/> indicates the local cluster, always displayed as `in-cluster`. The in-cluster server URL is always set to `https://kubernetes.default.svc/`.</li><li><img src="../../../images/icons/managed-cluster.png" display=inline-block/> indicates a managed cluster.</li> <li> <img src="../../../images/icons/add-cluster.png" display=inline-block/> select to add a new managed cluster.</li></ul> {:/} To view cluster components, select the cluster. To add and work with managed clusters, see [Adding external clusters to runtimes]({{site.baseurl}}/docs/runtime/managed-cluster). |
|**Health/Sync status** |The health and sync status of the runtime or cluster. {::nomarkdown}<ul><li><img src="../../../images/icons/error.png" display="inline-block"> indicates health or sync errors in the runtime, or a managed cluster if one was added to the runtime.</br> The runtime or cluster node is bordered in red and the name is colored red.</li> <li><img src="../../../images/icons/cf-sync-status.png" display=inline-block/> indicates that the runtime is being synced to the cluster on which it is provisioned.</li></ul> {:/} |
|**Search and View options** | {::nomarkdown}<ul><li>Find a runtime or its clusters by typing part of the runtime/cluster name, and then navigate to the entries found. </li> <li>Topology view options: Resize to window, zoom in, zoom out, full screen view.</li></ul> {:/}|

### Update Git tokens for runtimes

Provisioned runtimes require valid Git tokens at all times to authenticate Git actions by you as a user.  
>These tokens are specific to the user, and the same token can be used for multiple runtimes.

There are two different situations when you need to update Git tokens:  
* Update invalid, revoked, or expired tokens: Codefresh automatically flags runtimes with such tokens. It is mandatory to update the Git tokens to continue working with the platform. 
* Update valid tokens: Optional. You may want to update Git tokens, even valid ones, by deleting the existing token and replacing it with a new token.

The methods for updating any Git token are the same regardless of the reason for the update:  
* OAuth2 authorization, if your admin has registered an OAuth Application for Codefresh
* Git access token authentication, by generating a personal access token in your Git provider account with the correct scopes

**Before you begin**  
* To authenticate through a Git access token, make sure your token is valid and has [the required scopes]({{site.baseurl}}/docs/reference/git-tokens) 

**How to**
1. Do one of the following:
  * If you see a notification in the Codefresh UI about invalid runtime tokens,  click **[Update Token]**.
    The Runtimes page shows runtimes with invalid tokens prefixed by the key icon. Mouse over shows invalid token.
  * To update an existing token, go to [Runtimes](https://g.codefresh.io/2.0/account-settings/runtimes){:target="\_blank"}.
1. From the List view, select the runtime for which to update the Git token.
1. From the context menu with the additional actions at the top-right, select **Update Git Runtime Credentials**.

  {% include
 image.html
 lightbox="true"
 file="/images/runtime/update-git-runtime-token.png"
 url="/images/runtime/update-git-runtime-token.png"
 alt="Update Git runtime credentials"
 caption="Update Git runtime credentials"
  max-width="60%"
%}

{:start="4"}
1. Do one of the following: 
  * If your admin has set up OAuth access, click **Authorize Access to Git Provider**. Go to _step 5_.
  * Alternatively, authenticate with an access token from your Git provider. Go to _step 6_.

{:start="5"}  
1. For OAuth2 authorization:
  > If the application is not registered, you get an error. Contact your admin for help.  
  * Enter your credentials, and select **Sign In**.
  * If required, as for example if two-factor authentication is configured, complete the verification. 

    {% include 
      image.html 
      lightbox="true" 
      file="/images/administration/user-settings/oauth-user-authentication.png" 
      url="/images/administration/user-settings/oauth-user-authentication.png" 
      alt="Authorizing access with OAuth2" 
      caption="Authorizing access with OAuth2"
      max-width="30%" 
   %}

{:start="6"} 
1. For Git token authentication, expand **Advanced authorization options**, and then paste the generated token in the **Git runtime token** field.

1. Click **Update Credentials**.


### Configure SSH for runtimes
By default, Git repositories use the HTTPS protocol. You can also use SSH to connect Git repositories by entering the SSH private key.

>When SSH is configured for a runtime, when creating/editing Git-Source applications, you can select HTTPS OR SSH as the protocol to connect to the Git repository. See [Repository URL in Application Source definitions]({{site.baseurl}}/docs/deployment/create-application/#source).

**SSH keys**  
For more information on generating SSH private keys, see the official documentation:
* [GitHub](https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent){:target="\_blank"}
* [GitLab](https://docs.gitlab.com/ee/ssh/#generating-a-new-ssh-key-pair){:target="\_blank"}
* [Bitbucket](https://confluence.atlassian.com/bitbucket/set-up-an-ssh-key-728138079.html){:target="\_blank"}
* [Azure](https://docs.microsoft.com/en-us/azure/devops/repos/git/use-ssh-keys-to-authenticate?view=azure-devops&tabs=current-page){:target="\_blank"}

**Before you begin**  
Copy the SSH private key for your Git provider  


**How to**  
1. In the Codefresh UI, make sure you are in [Runtimes](https://g.codefresh.io/2.0/account-settings/runtimes){:target="\_blank"}.
1. From the **List View**, select the runtime for which to configure SSH.
1. From the context menu with the additional actions at the top-right, select **Update Git Runtime Credentials**. 

  {% include
 image.html
 lightbox="true"
 file="/images/runtime/update-git-runtime-token.png"
 url="/images/runtime/update-git-runtime-token.png"
 alt="Update Git runtime credentials"
 caption="Update Git runtime credentials"
  max-width="60%"
%}

{:start="4"}
1. Expand **Connect Repo using SSH**, and then paste the raw SSH private key into the field.

{% include
 image.html
 lightbox="true"
 file="/images/runtime/configure-ssh-for-runtimes.png"
 url="/images/runtime/configure-ssh-for-runtimes.png"
 alt="Update Git runtime credentials"
 caption="Update Git runtime credentials"
  max-width="40%"
%}

{:start="5"}
1. Click **Update Credentials**.

### Reset shared configuration repository
Codefresh creates the [shared configuration repository]({{site.baseurl}}/docs/reference/shared-configuration) when you install the first hybrid or hosted GitOps runtime for your account, and uses it for all runtimes you add to the same account.

If needed, you can reset the location of the shared configuration repository in your account and re-initialize it. For example, when moving from evaluation to production.  
Uninstall all the existing runtimes in your account, and then run the reset command. On the next installation, Codefresh re-initializes the shared configuration repo.
 

>Reset shared configuration repo is supported from CLI v0.1.18 and higher.


**Before you begin**   
[Uninstall every runtime in the account](#uninstall-provisioned-runtimes)

**How to**  
* Run:  
  `cf config --reset-shared-config-repo`



### Upgrade Codefresh CLI
The Codefresh CLI automatically self-checks its version, and if a newer version is available, prints a banner with the notification.  

 {% include
    image.html
  lightbox="true"
  file="/images/runtime/cli-upgrade-banner.png"
  url="/images/runtime/cli-upgrade-banner.png"
  alt="Upgrade banner for Codefresh CLI"
  caption="Upgrade banner for Codefresh CLI"
  max-width="40%"
  %}

You can upgrade to a specific version if you so require, or download the latest version to an output folder to upgrade at your convenience.


* Do any of the following:
  * To upgrade to the latest version, run:  
    `cf upgrade`
  * To upgrade to a specific version, even an older version, run:  
    `cf upgrade --version v<version-number>`  
    where:  
    `<version-number>` is the version you want to upgrade to.
  * To download the latest version to an output file, run:  
    `cf upgrade --version v<version-number> -o <output-file>`  
    where:   
    `<output-file>` is the path to the destination file, for example, `/cli-download`.
  

### (Hybrid) Upgrade provisioned runtimes

Upgrade provisioned hybrid runtimes to install critical security updates or to install the latest version of all components. Upgrade a provisioned hybrid runtime by running a silent upgrade or through the CLI wizard.  
If you have managed clusters for the hybrid runtime, upgrading the runtime automatically updates runtime components within the managed cluster as well.

> When there are security updates, the UI displays the alert, _At least one runtime requires a security update_. The Version column displays an _Update Required!_ notification.  

> If you have older runtime versions, upgrade to manually define or create the shared configuration repo for your account. See [Shared configuration repo]({{site.baseurl}}/docs/reference/shared-configuration/).


**Before you begin**  
For both silent or CLI-wizard based upgrades, make sure you have:  

* The [latest version of the Codefresh CLI](#upgrade-codefresh-cli)  
  <!--Run `cf version` to see your version and [click here](https://github.com/codefresh-io/cli-v2/releases){:target="\_blank"} to compare with the latest CLI version. --> 
* A valid Git token with [the required scopes]({{site.baseurl}}/docs/reference/git-tokens) 

**Silent upgrade**  

* Pass the mandatory flags in the upgrade command:  

  `cf runtime upgrade <runtime-name> --git-token <git-token> --silent`
  where:  
  `<git-token>` is a valid runtime token with the `repo` and `admin-repo.hook` scopes.

**CLI wizard-based upgrade**

1. In the Codefresh UI, make sure you are in [Runtimes](https://g.codefresh.io/2.0/account-settings/runtimes){:target="\_blank"}.
1. Switch to either the **List View** or to the **Topology View**.  
1. **List view**:  
  * Select the runtime name.
  * To see all the commits to the runtime, in the Version column, mouse over **Update Available!**, and select **View Complete Change Log**.
  * On the top-right, select **Upgrade**.
  
  {% include
    image.html
  lightbox="true"
  file="/images/runtime/runtime-list-view-upgrade.png"
  url="/images/runtime/runtime-list-view-upgrade.png"
  alt="List View: Upgrade runtime option"
  caption="List View: Upgrade runtime option"
  max-width="30%"
  %}

  **Topology view**:  
  Select the runtime cluster, and from the panel, select the three dots and then select **Upgrade Runtime**.
  {% include
 image.html
 lightbox="true"
 file="/images/runtime/runtiime-topology-upgrade.png"
 url="/images/runtime/runtiime-topology-upgrade.png"
 alt="Topology View: Upgrade runtime option"
 caption="Topology View: Upgrade runtime option"
  max-width="30%"
%}

{:start="4"}

1. If you have already installed the Codefresh CLI, in the Install Upgrades panel, copy the upgrade command.

  {% include
 image.html
 lightbox="true"
 file="/images/runtime/install-upgrades.png"
 url="/images/runtime/install-upgrades.png"
 alt="Upgrade runtime"
 caption="Upgrade runtime panel"
  max-width="30%"
%}  

{:start="5"}
1. In your terminal, paste the command, and do the following:
  * Update the Git token value.
  * To manually define the shared configuration repo, add the `--shared-config-repo` flag with the path to the repo.
1. Confirm to start the upgrade.



<!---### (Hybrid) Migrate runtimes
To migrate a tunnel-based runtime to an ingress-based one or vice-versa, you must uninstall the existing runtime and then install a a new tunnel- or ingress-based runtime. 
You can retain the installation repo used to install the current runtime. Though empty after uninstalling the ingress-less The new installation creates the new manifests in this re


>Before uninstalling the ingress-less runtime, you can save specific patches in a temporary location or retrieve the same from the Git history, and re-apply them after installing the ingress-based runtime.

**Before you begin**  
* Make sure the ingress controller for the new runtime meets [requirements and is configured as needed]({{site.baseurl}}/docs/runtime/requirements/)

**How to**  
1. Uninstall the ingress-less runtime, as described in [Uninstall provisioned runtimes](#uninstall-provisioned-runtimes) in this article.
2. Install the new ingress-based runtime, as described in [Install hybrid runtimes]({{site.baseurl}}/docs/runtime/installation/).

--->
### Uninstall provisioned runtimes

Uninstall provisioned hybrid and hosted runtimes that are not in use.  Uninstall a runtime by running a silent uninstall, or through the CLI wizard.  
> Uninstalling a runtime removes the Git Sources and managed clusters associated with the runtime.

**Before you begin**  
For both types of uninstalls, make sure you have:  

* The latest version of the Codefresh CLI
* A valid runtime Git token
* The Kube context from which to uninstall the provisioned runtime

**Silent uninstall**  
Pass the mandatory flags in the uninstall command:  
  `cf runtime uninstall <runtime-name> --git-token <git-token> --silent`  
  where:  
  `--git-token` is a valid runtime token with the `repo` and `admin-repo.hook` scopes.  

**CLI wizard uninstall**  

1. In the Codefresh UI, make sure you are in [Runtimes](https://g.codefresh.io/2.0/account-settings/runtimes){:target="\_blank"}.
1. Switch to either the **List View** or to the **Topology View**.
1. **List view**: On the top-right, select the three dots and then select **Uninstall**.

  {% include
 image.html
 lightbox="true"
 file="/images/runtime/uninstall-location.png"
 url="/images/runtime/uninstall-location.png"
 alt="List View: Uninstall runtime option"
 caption="List View: Uninstall runtime option"
  max-width="30%"
%}

**Topology view**: Select the runtime node, and from the panel, select the three dots and then select **Uninstall Runtime**.
  {% include
 image.html
 lightbox="true"
 file="/images/runtime/runtime-topology-uninstall.png"
 url="/images/runtime/runtime-topology-uninstall.png"
 alt="Topology View: Uninstall runtime option"
 caption="Topology View: Uninstall runtime option"
  max-width="30%"
%}

{:start="4"}

1. If you already have the latest version of the Codefresh CLI, in the Uninstall Codefresh Runtime panel, copy the uninstall command.

  {% include
 image.html
 lightbox="true"
 file="/images/runtime/uninstall.png"
 url="/images/runtime/uninstall.png"
 alt="Uninstall Codefresh runtime"
 caption="Uninstall Codefresh runtime"
  max-width="40%"
%}

{:start="5"}

1. In your terminal, paste the command, and update the Git token value.
1. Select the Kube context from which to uninstall the runtime, and then confirm the uninstall.
1. If you get errors, run the uninstall command again, with the `--force` flag.





### Related articles
[Monitor provisioned hybrid runtimes]({{site.baseurl}}/docs/runtime/monitoring-troubleshooting/)  
[Add Git Sources to runtimes]({{site.baseurl}}/docs/runtime/git-sources/)  
[Add external clusters to runtimes]({{site.baseurl}}/docs/runtime/managed-cluster/)  

