---
title: "Access control"
description: ""
group: administration
toc: true

---
Access control defines the access policy for resources within an enterprise.   
In CSDP, access control to an entity is derived from the entity type, that can be categorized into one of the following:

* **GitOps-controlled entities**  
  These are entities whose entire lifecyle - creation, update, and deletion, are fully GitOps-controlled.  
  Examples of such entities in CSDP include:
  * Runtimes
  * Git Sources
  * Pipelines comprising Argo Workflow/Event resources such as the Workflow Template, Sensor, Event Sources
  * Applications comprising Argo CD/Rollouts resources project, applicationSet, application, rollout)

* **Non-GitOps-controlled entities**  

  These are entities reported to CSDP as built artifacts, but not GitOps-controlled.
  * Images

* **Account-configuration entities (currently non-GitOps-controlled)**  

  These are entities whose state is not stored in a Git repository for now.
  * Account configuration collaborators
  * Account configuration security
  * Account configuration Single Sign-On (SSO)
  * Billing


### GitOps-controlled entities
CSDP stores the state of your account entities based on GitOps principles and policies. 

#### Write permissions
Users with write permissions can access and manage files directly in the Git repository. Any action on the file such as create, update, or delete, is immediately reflected in the user account.  

Any user action via a CSDP client (UI or CLI), on a GitOps-controlled resource, is impersonated with the user's Git permissions. If the user does not have permissions for an action in Git, then the user is automatically denied access to the same action from a CSDP client.  

For CSDP to impersonate the user, the user must provide Git credentials for every runtime. The credentials are securely stored by the CSDP application proxy.  
The CSDP application proxy uses these credentials:
* For Git-provider operations
* To update CSDP with the read/write permissions to all existing repositories linked to the Git Source defined for a runtime. The CSDP client can perform client-side validations.

To add your Git personal token, in the CSDP UI, go to [user settings](https://g.codefresh.io/2.0/user-settings).

{% include
image.html
lightbox="true"
file="/images/administration/access-control/pat.png"
url="/images/administration/access-control/pat.png"
alt="Add personal access token"
caption="Add personal access token"
max-width="100%"
%}

#### Read view permissions
CSDP enforces read permissions by checking if the user has Git permissions to view the Kubernetes manifest in the repository.  
Read-view permissions to entities created dynamically from changes in resource state are inherited from the permissions of the parent entity.

From the user's perspective, this means that:

* If the user does not have read-view permissions from the Git provider to the Sensor's Kubernetes manifest, the user does not have visibility into pipelines.  
  Workflow entities that are dynamically created, derive their read-view permissions from pipeline permissions. 

* Notifications are displayed only for resources with read permissions.


> Currently, we do not enforce Analytics views according to read permissions for pipelines. We will be enforcing this policy in the upcoming releases.

#### Write operations on dynamically-created entities
These are operations users can perform on dynamically-created entities, such as workflows, for example. Typically, the permissions for such entities are derived from those of the parent entity.  

For now, all users with view permissions, can also terminate and retry workflows. 


### Non-GitOps controlled entities
Users can for now view all `image` entity types. These are resources reported to CSDP as built artifacts, but not stored using the GitOps approach.

### Account-configuration for non-GitOps controlled entities
All account-configuration entities you have access to are listed in your account settings, and are exposed only to account admins.  

When adding a user account, you can assign the `admin` role to the user. The `admin` role automatically enables all account-configurations.

### Runtime account-configuration 
Runtime configuration is also exposed in the account settings dedicated area and only exposed to admins but is fully controlled via the GitOps approach after installation. <br>

Users with write permissions to the runtime installation repository in Git can make changes to the runtime, and create/delete/update Git Sources defined for that runtime.
We are at present exposing the runtime configuration under the account settings only to account admins.   
Be aware though that these can also be changed directly through Git by users who are not admin users in CSDP. <br>

For now, CSDP admin users can see all runtimes and Git Sources even if they don't have read permissions to the underlying Git repository.


### Upcoming enhancements to access control
We are continuing to enhance our access control model by adding another layer to provide the ability to define:
* Permissions on write operations for entities that are non-GitOps controlled, such as account configuration and workflow operations
* Read permissions for entities that are completely non-GitOps controlled
* A more granular permission model for entities that are GitOps-controlled, but without sufficient access control policies in place
* A more granular permission model for dynamic resources that are non-GitOps controlled, but created from a GitOps-controlled entity, for example, workflows
