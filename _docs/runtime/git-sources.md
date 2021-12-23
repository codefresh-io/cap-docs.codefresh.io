---
title: "DRAFT: Git Source management"
description: ""
group: runtime
toc: true
---

A Git Source in CSDP is an ArgoCD application that references a Git repository. CSDP uses Git Sources to sync Argo Workflow and Argo Events resources to the cluster. 
Git Sources are always associated with a specific runtime. A single runtime can have multiple Git Sources.

When you provision a Codefresh runtime, a default Git Source is automatically created. This Git Source houses the Workflow Templates and Sensors. 

(NIMA: Itai, please add more meat on why we need them, when you add more, how CF uses them)

### View Git Sources
Git Sources are displayed when you drill down on a runtime. 

1. Go to **Runtimes**.
1. Click a runtime name, and then select the **Git Sources** tab.  

  (SCREENSHOT)

{:start="3"}
1. To go to the repo linked to the Git Source, click the link in the Repo column.

### Create a new Git Source
1. Drill down into the runtime for which to add a Git Source, and select Git Sources.
1. Select **+Create Git Source**.  

(SCREENSHOT)

{:start="3"}
1. If you have already installed the Codefresh CLI, continue with the Git Source creation on the lower part of the panel.
  >If you created a new GitOps repo for the Git Source, make sure that the Git token has admin rights to the new repo.


### What to read next
[Monitor and manage runtimes]({{site.baseurl}}/docs/runtime/monitor-manage-runtimes/)
