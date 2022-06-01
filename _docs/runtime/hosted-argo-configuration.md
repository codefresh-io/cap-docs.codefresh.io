---
title: "Inatall and configure Hosted Argo CD"
description: ""
group: runtime
toc: true
---


If you have signed-in to our SaaS offering, get started right away.
Hosted Argo CD is Codefrsesh's SaaS version, where the runtime installation is manag

 version is the easiest way to start using Codefresh as it is fully managed and runs 100% on the cloud. All maintenance and updates are executed by the Codefresh DevOps team.


 1. Install your hosted runtime
    Start installing your hosted runtime with a single click and Codefresh will complete the installation without furhter intervention on your part. 

2. Connect to a Git provider
   Connect to a Git provider to store and manage your configuration, infrastructure, and application code. Argo CD syncs the resources from the Git repositories to your cluster, with the git repository is the source of truth.
   Set up GitHub integration (link)
   Authorize access to Git provider Update runtime token
   Select Organization for Git Configuration repos

 

   After successsful integration, Codefresh creates two Git repositories in the Git Organization you select. Make sure the organization has the correct permissions.
   What is shared configuration
The Shared Configuration repo, as it's name indicates, stores the configuration shared across multiple runtimes. A Git organization canhave a single such repo. Runtimes created for that account can point to this repo to retrieve and use the configuration.  already have.


3. Connect a Kubernetes cluster
   Connect a destination cluster and register it as a managed cluster. Your applications and configurations are deployed to the cluseter.
   See ?????

4. Optional Create application

5. Connect CI 





