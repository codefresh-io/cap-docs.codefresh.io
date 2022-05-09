---
title: "Create an application"
description: ""
group: deployment
toc: true
---



Create applications that are fully GitOps compliant in the Codefresh UI. Codefresh integrates with Argoo CD"s application CRD The application creation flow is filly GitOps compliant form generating the application manifest to commit the changes and then deployoin


For how-to information, see [How to: Create an application](#how-to-create-an-Argo-CD-application)



### Application definitions
Application creation in Codefresh involves application definitions and the application configuration settings.

### General configuration settings
General settings define the source, destination, and sync policies for the application.  

##### Source 
The Git repository to be tracked for changes to the application's source code.  
{::nomarkdown}<ul> <li><b>ArgoCD Project</b>: The project group to which the application belongs. A project is useful to enforce restrictions on permitted sources and targets for applications, and roles. If not defined, the application is automatically assigned to the `default` project, which is created automatically by Argo CD and has no restrictions. </br>For more information, see Argo CD's documentation on <a href="https://argo-cd.readthedocs.io/en/stable/user-guide/projects/#projects" target="\_blank">Projects</a>.</li> <li><b>Repository URL</b>: The repository where the application manifests are located. Git repos and Helm package repos are supported. and can be a Git repository or as a Helm package. If the the Argo CD project is not the `default` project, make sure that the repo has the correct access roles for your application.</li> <li><b>Revision and Path</b>: Applies to Git repositories. </li><li><b>Chart</b>: Applies to Helm repositories. The name of the Helm package with all the resource definitions for the application, and the version. (Is this the same as release?)</li></ul> {:/}   

##### Destination
The cluster and namespace to which to deploy the application.  
{::nomarkdown}<ul><li><b>Cluster</b>: The cluster to which to deploy the application, defined as a <b>URL</b> or as the user-defined display <b>NAME</b>.</li> <li><b>Namespace</b>: The namespace in the cluster to which to deploy the application.</li> </ul> {:/}
 
##### Sync Settings
{::nomarkdown}<b>Sync Policy</b>: The synchronization policy to apply when syncing the desired state to the actual state in the cluster.</br><ul><li><b>Manual</b>: Manually sync the changes from the Argo CD UI. </li><li><b>Automatic</b>: Automatically sync changes when there are differences between the desired state in Git and the actual state in the cluster, and the following options when selected:<ul><li><b>Prune resources</b>:When selected, removes legacy resources that do not exist currently in Git. </li><li><b>Self heal</b>: When selected, always enforces a sync to the desired state in Git, if and when there is a change to the live state in the cluster. See <a href="https://argo-cd.readthedocs.io/en/stable/user-guide/auto_sync/#automatic-self-healing" target="_blank">Automatic self-healing</a>.</li></li></ul> {:/}
  
{::nomarkdown}<b>Sync Options</b>: Common to both manual and automatic sync policies.</br><ul><li><b>Skip schema validation</b>:</li><li><b>Auto-create namespace</b>: When selected, automatically create the namespace if the specified namespace does not exist in the cluster.</li><li><b>Prune last</b>: When selected, removes those resources that do not exist in the currently deployed version during the final wave of the sync operation. See <a hef="https://argo-cd.readthedocs.io/en/stable/user-guide/sync-options/#prune-last" target="_blank">Prune last</a>.</li><li><b>Apply out of sync only</b>: When selected, sync only those resources in the application that have been changed and are `OutOfSync`, instead of syncing every resource regardless of state. This option is useful to reduce load and save time when you have thousands of resources in an application. See <a href="https://argo-cd.readthedocs.io/en/stable/user-guide/sync-options/#selective-sync" target="_blank">Selective Sync</a>.</li></ul> {:/}
  
{::nomarkdown}<b>Prune propagation policy</b>:</br>Defines how resources are pruned, applying Kubernetes cascading deletion prune policies. 
For detailed information, see <a href="https://kubernetes.io/docs/concepts/architecture/garbage-collection/#cascading-deletion" target="_blank">Kubernetes - Cascading deletion</a>.</br><ul><li><b>Foreground</b>: The default prune propagation policy used by Argo CD. With this policy, Kubernetes changes the state of the owner resource to `deletion in progress`, until the controller deletes the dependent resources and finally the owner resource itself. </li><li><b>Background</b>: When selected, Kubernetes deletes the owner resource immediately, and then deletes the dependent resources in the background.</li><li><b>Orphan</b>: When selected, Kubernetes deletes the dependent resources that remain orphaned after the owner resource is deleted.</li></ul> </br>{:/}

All Prune propagation policies can be used with:  
**Replace**: When selected, Argo CD executes `kubectl replace` or `kubectl create`, instead of the default `kubectl apply` to enforce the changes in Git. This action will potentially recreate resources and should be used with care. See <a href="https://argo-cd.readthedocs.io/en/stable/user-guide/sync-options/#replace-resource-instead-of-applying-changes" target="_blank">Replace Resource Instead Of Applying Changes</a>.   
**Retry**: Defines if to retry a failed sync operation, and the settings that control the retry.  
Limit is the maximum number of sync retries, with the Duration of each retry attempt in seconds, minutes, or hours, the Max Duration permitted for each retry, and the Factor by which to multiply the Duration in the event of a failed retry. A factor of 2 for example, attempts the second retry in 2 X 2 seconds, where 2 seconds is the Duration. 

### Advanced configuration settings
Advanced settings define the tool used to create the application, and related settings.


##### Type of Application
The tool used to create the application's manifests.  

{::nomarkdown}<ul><li><b>Directory</b>: A `directory` application, which is the default application type in Argo CD.</br>To include subdirectories, select <b>Directory recurse</b>. </br></li><li><b>Helm</b>: When using Helm charts to create the application, in <b>Values files</b>, specify one or more `values.yaml` files that store the parameters. </br><b>Values</b> Optional. Define overrides for the values in the `values.yaml` files. Argo CD will overide the values defined for the specified parameters.</li><li><b>Kustomize</b>: When using Kustomize to create the application, configure the following settings:<br><ul><li><b>Version</b>: The version of Kustomize used to create the application.</li><li><b>Name Prefix</b>: The prefix to be appended to the resources of the application.</li><li><b>Name Suffix</b>: The suffix to be appended to the resources of the application.</li></ul> </li><li><b>Plugin</b>: When using plugins to create the application, configure the following settings:<br><ul><li><b>Name</b>: The name of the Plugin used to create the application.</li><li><b>External Variables</b>: The variables to use in the application.</li></ul></li>{:/} 

### How to: create an Argo CD application
Create a new application from the Applications dashboard with the Add Application wizard. 
Work either in the YAML or Form mode, and toggle between the modes as you need.

**Before you begin**  

Review [Application definitions and configuration settings](#Application configuration settings)

**How to**  
1. In the Codefresh UI, go to [Applications](https://g.codefresh.io/2.0/applications-dashboard?sort=desc-lastUpdated){:target="\_blank"}.
1. Select **Add Application** on the top-right.
1. In the Add Application panel, add definitions for the application:
  * Application name: Must be unique within the cluster.
  * Runtime: The runtime to associate with the application.  
  * YAML filename: The name of the application's configuration manifest, assigned on commit to Git. By default, the manifest is assigned the application name. Change the name as required.

  >The application definitions cannot be changed after you continue to the Configuration settings.

{% include 
   image.html 
   lightbox="true" 
   file="/images/applications/add-app-definitions.png" 
   url="/images/applications/add-app-definitions.png" 
   alt="Add Application panel" 
   caption="Add Application panel"
   max-width="30%" 
   %} 

{:start="4"}
1. Select **Next** to go to the Configuration tab.  
  By default you are in Form mode. You can toggle between the Form and YAML modes as you define the application's configuration settings.
1. Define the **General** settings for the application. 

{% include 
   image.html 
   lightbox="true" 
   file="/images/applications/add-app-general-settings.png" 
   url="/images/applications/add-app-general-settings.png" 
   alt="Add Application: General settings" 
   caption="Add Application: General settings"
   max-width="30%" 
   %} 

{:start="5"}
1. Define the **Advanced** settings for the application. 

{% include 
   image.html 
   lightbox="true" 
   file="/images/applications/add-app-general-settings.png" 
   url="/images/applications/add-app-general-settings.png" 
   alt="Add Application: Advanced settings" 
   caption="Add Application: Advanced settings"
   max-width="30%" 
   %} 

{:start="6"}   
1. To commit all your changes, select **Commit**.  
  The Commit form is displayed with the application's definition on the left, and the manifest configuration you defined on the right.
1. Enter the path to the **Git Source** to which to commit the application configuration manifest.

{% include 
   image.html 
   lightbox="true" 
   file="/images/applications/add-app-final-commit.png" 
   url="/images/applications/add-app-final-commit.png" 
   alt="Add Application: Final commit" 
   caption="Add Application: Final commit"
   max-width="30%" 
   %} 

{:start="8"} 
1. Add a commit message and then select **Commit** on the bottom-right of the panel.


Your application is first committed to Git and then synced to the cluster. 
