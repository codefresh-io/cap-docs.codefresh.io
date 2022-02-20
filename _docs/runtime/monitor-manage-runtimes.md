---
title: "Management"
description: ""
group: runtime
toc: true
---


The **Runtimes** page displays all the runtimes deployed across your environment.  
Here you can monitor and manage the runtimes:
* Identify errors through the Sync Status column
* Configure browsers to allow access for insecure runtimes
* Upgrade runtimes
* Drill down into specific runtimes for further analysis
* Manage Git Sources associated with runtimes
* Uninstall runtimes
 

### Allow insecure runtimes
If at least one of your runtimes was installed in insecure mode (without an SSL certificate for the ingress controller from a CA), the CSDP UI displays the _At least one runtime was installed insecure mode_ alert.
{% include 
	image.html 
	lightbox="true" 
	file="/images/runtime/runtime-insecure-alert.png" 
	url="/images/runtime/runtime-insecure-alert.png" 
	alt="Insecure runtime installation alert" 
	caption="Insecure runtime installation alert"
  max-width="100%" 
%} 

All you need to do is to configure the browser to trust the URL and receive content.

1. In the CSDP UI, go to the [**Runtimes**](https://g.codefresh.io/2.0/account-settings/runtimes) page.
1. Select **View Runtimes** to the right of the alert.  
  You are taken to the Runtimes page, where you can see insecure runtimes, tagged as **Allow Insecure**.
  {% include 
	image.html 
	lightbox="true" 
	file="/images/runtime/runtime-insecure-steps.png" 
	url="/images/runtime/runtime-insecure-steps.png" 
	alt="Insecure runtimes in Runtime page" 
	caption="Insecure runtimes in Runtime page"
  max-width="40%" 
%} 
{:start="3"}
1. For each insecure runtime, select **Allow Insecure**, and when the browser prompts you to allow access, do as relevant:
  * Chrome: Click **Advanced** and then **Proceed to site**.
  * Firefox: Click **Advanced** and then **Accept the risk and continue**.
  * Safari: Click **Show Certificate**, and then select **Always allow content from site**.
  * Edge: Click **Advanced**, and then select **Continue to site(unsafe)**.

### Upgrade runtimes
Upgrade existing runtimes to install critical security updates or the latest version of all components. Upgrade through the CLI wizard, or by running a silent upgrade.  

When there are security updates, the CSDP UI displays the alert, _At least one runtime requires a security update_.   
If there are newer versions of the runtime, the Version column displays an _Update Available!_ notification, with a link to the change log.

> When there are newer versions, the Upgrade option is automatically displayed in the Runtimes page, and also when you drill down into specific runtimes. 

**Before you begin**  
For both types of upgrades, make sure you have:  

* The latest version of the CSDP CLI 
* A valid Git token 
* The Kube context from which to uninstall the runtime

**Silent upgrade**  

Pass the mandatory flags in the upgrade command:  
  `cf runtime upgrade <runtime-name> --repo <git-repo> --git-token <git-token> --silent`  

**CLI wizard-based upgrade**   

1. In the CSDP UI, make sure you are in the [**Runtimes**](https://g.codefresh.io/2.0/account-settings/runtimes) page.
1. To see all the commits to the runtime, in the Version column, mouse over **Update Available!**, and select **View Complete Change Log**.
  
  {% include 
	image.html 
	lightbox="true" 
	file="/images/runtime/change-log-popup.png" 
	url="/images/runtime/change-log-popup.png" 
	alt="Link to runtime change log" 
	caption="Link to runtime change log"
  max-width="30%" 
%} 

  Here's an example of the change log:

      {% include 
	image.html 
	lightbox="true" 
	file="/images/runtime/changelog.png" 
	url="/images/runtime/changelog.png" 
	alt="Change log" 
	caption="Change log"
  max-width="30%" 
%} 

{:start="3"}
1. On the top-right, select **Upgrade**. 
1. If you have already installed the Codefresh CLI, continue with the runtime upgrade instructions in the lower part of the Install Upgrades panel.

  {% include 
	image.html 
	lightbox="true" 
	file="/images/runtime/install-upgrades.png" 
	url="/images/runtime/install-upgrades.png" 
	alt="Upgrade runtime" 
	caption="Upgrade runtime"
  max-width="30%" 
%}  

### Drill down into a runtime
Drill down into a specific runtime to view its components, Git Sources, and troubleshoot sync and health issues.

1. In the CSDP UI, make sure you are in the [**Runtimes**](https://g.codefresh.io/2.0/account-settings/runtimes) page.
1. Select a runtime name to see the list of runtime components, Git Sources, and their status.  

  {% include 
	image.html 
	lightbox="true" 
	file="/images/runtime/drilldown-on-runtime.png" 
	url="/images/runtime/drilldown-on-runtime.png" 
	alt="Runtime Components and Git Sources in runtime" 
	caption="Runtime Components and Git Sources in runtime"
  max-width="40%" 
%}


### Troubleshoot health and sync errors 
The runtime name in red, prefixed by ![](/images/runtime/icon-ExclamationCircle.png?display=inline-block) indicates either health or sync errors. 

**Health errors**  
Health errors are generated by Argo CD and by CSDP for runtime components. 


**Sync errors**  
Sync errors indicate have an **Out of sync** status in Sync Status column. They are related to discrepancies between the desired and actual state of a runtime component or one of the Git sources associated with the runtime.  

**View errors**  
Drill down into the runtime, and then select **Errors Detected**.
 


### Uninstall runtimes
Uninstall CSDP runtimes that are not in use.  Uninstall a runtime through the CLI wizard, or by running a silent uninstall. 
 

**Before you begin**  
For both types of uninstalls, make sure you have:  

* The latest version of the CSDP CLI 
* A valid Git token 
* The Kube context from which to uninstall the runtime

**Silent uninstall**  
Pass the mandatory flags in the uninstall command:  
  `cf runtime uninstall <runtime-name> --repo <git-repo> --git-token <git-token> --silent`  

**CLI wizard-based uninstall**  

1. In the CSDP UI, make sure you are in the [**Runtimes**](https://g.codefresh.io/2.0/account-settings/runtimes) page.
1. Drill down into the runtime to uninstall.
1. On the top-right, select the three dots and then select ![](/images/runtime/icon-Download.png?display=inline-block) **Uninstall**.

  {% include 
	image.html 
	lightbox="true" 
	file="/images/runtime/uninstall-location.png" 
	url="/images/runtime/uninstall-location.png" 
	alt="Uninstall runtime option" 
	caption="Uninstall runtime option"
  max-width="40%" 
%} 

{:start="4"}
1. If you already have the latest version of the CSDP CLI, in the Uninstall Codefresh Runtime panel, copy the uninstall command.

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
1. In your terminal, paste the command, update the Git token value..
1. Select the Kube context from which to uninstall runtime, and then confirm the uninstall prompt.
1. If you get errors, run the uninstall command again, with the `--force` flag. 


### What to read next
[Manage Git Sources]({{site.baseurl}}/docs/runtime/git-sources/)
