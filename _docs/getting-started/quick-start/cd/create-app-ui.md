




**How to**  
1. In the Codefresh UI, go to [Applications](https://g.codefresh.io/2.0/applications-dashboard?sort=desc-lastUpdated){:target="\_blank"}.
1. Select **Add Application** on the top-right.
1. In the Add Application panel, add definitions for the application:
  * **Application name**: For the quick start, `codefresh.guestbook`.
  * **Runtime**: The runtime to associate with the application.  `argocd` .  
  * **YAML filename**: The name of the application's configuration manifest, assigned on commit to Git. By default, the manifest is assigned the application name.

  >The application definitions cannot be changed after you continue to the Configuration settings.

{% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/quick-start/cdops-add-app-settings.png" 
   url="/images/getting-started/quick-start/cdops-add-app-settings.png" 
   alt="Add Application panel" 
   caption="Add Application panel"
   max-width="50%" 
   %} 

{:start="4"}
1. Select **Next** to go to the Configuration tab.  
  By default you are in Form mode. You can toggle between the Form and YAML modes as you define the application's configuration settings. You can edit the YAML manifest.
1. Define the **General** settings for the application: 
  * **Repository URL**: The URL to the repo in Git where you created the YAML resource files for the application.
  * **Revision**: The branch in Git with the resource files.
  * **Path**: The folder in Git with the resource files.
  * **Namespace**: Optional. Create a new namespace for the application. For the quick start, we'll create a namespace for the application, `quick-start`. 
  * **Sync Policy**: Change to **Automatic**, and select **Prune resources** to automatically remove unused resources.
  * **Sync Options**: Select **Auto-create namespace** to ensure that our namespace is created if it doesn't exist. 
 
{% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/quick-start/cdops-add-app-configuration.png" 
   url="/images/getting-started/quick-start/cdops-add-app-configuration.png" 
   alt="Add Application Quick Start: General settings" 
   caption="Add Application Quick Start: General settings"
   max-width="70%" 
   %} 


{:start="3"}
1. Leave the default **Advanced** settings. 
{:start="7"}   
1. To commit all your changes, select **Commit**.  
  The Commit form is displayed with the application's definition on the left, and the read-only version of the manifest with the configuration settings you defined on the right.
1. Enter the path to the **Git Source** to which to commit the application configuration manifest.

{% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/quick-start/cdops-add-app-commit.png" 
   url="/images/getting-started/quick-start/cdops-add-app-commit.png" 
   alt="Add Application Quick Start: Commit to Git" 
   caption="Add Application Quick Start: Commit to Git"
   max-width="70%" 
   %} 

{:start="4"} 
1. Add a commit message and then select **Commit** at the bottom-right of the panel.

1. View the application in the [Applications dashboard](https://g.codefresh.io/2.0/applications-dashboard?sort=desc-lastUpdated){:target="\_blank"}.  
  You may have to wait for a few seconds until the application is synced to the cluster.

  {% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/quick-start/cdops-add-app-dashboard.png" 
   url="/images/getting-started/quick-start/cdops-add-app-dashboard.png" 
   alt="Application dashboard with new application" 
   caption="Application dashboard with new application"
   max-width="70%" 
   %} 

The final step is to make a change in the application manifest to force a rollout. 