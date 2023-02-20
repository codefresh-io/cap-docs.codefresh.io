---
title: "Download/upgrade Codefresh CLI"
description: ""
canonical_url: 'https://codefresh.io/docs/docs/installation/gitops/upgrade-gitops-cli/'
group: clients
toc: true
---

You need the Codefresh CLI to install Codefresh runtimes. For the initial download, you also need to generate the API key and create the API authentication context, all from the UI.  
If upgrades are needed, the CLI notifies you through a banner, and you can use the existing API credentials. 

### Download Codefresh CLI
Downloading the Codefresh CLI requires you to select the download mode and OS, generate an API key, and authentication context.
1. Do one of the following:
  * For first-time installation, go to the Welcome page, select **+ Install Runtime**.
  * If you have provisioned a hybrid/hosted runtime, in the Codefresh UI, go to [Runtimes](https://g.codefresh.io/2.0/account-settings/runtimes){:target="\_blank"}, and select **+ Add Runtime**.
1. Download the Codefresh CLI:
  * Select one of the methods. 
  * Generate the API key and create the authentication context. 
    {% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/quick-start/quick-start-download-cli.png" 
   url="/images/getting-started/quick-start/quick-start-download-cli.png" 
   alt="Download CLI to install runtime" 
   caption="Download CLI to install runtime"
   max-width="50%" 
   %} 

### Upgrade Codefresh CLI
The Codefresh CLI automatically self-checks its version, and if a newer version is available, prints a banner with the notification.  

 {% include
    image.html
  lightbox="true"
  file="/images/runtime/cli-upgrade-banner.png"
  url="/images/runtime/cli-upgrade-banner.png"
  alt="Upgrade banner for Codefresh CLI"
  caption="Upgrade banner for Codefresh CLI"
  max-width="50%"
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


### Related articles
[Set up hosted (Hosted GitOps) environment]({{site.baseurl}}/docs/runtime/hosted-runtime)  
[Install hybrid runtimes]({{site.baseurl}}/docs/runtime/installation)  
