---
title: ""
description: ""
group: getting-started
toc: true
---

## Classic version navigation

### Avatar options
|  CLASSIC             |                               |  |NEW              |             |
|                       | Menu item                    |  |                 |             | 
| -------------------  | -------------------           |  | ---------------- | ---------  | 
|Avatar                 |                              |  |                  |            |     
|                       |Account Settings              |  |                  | Moved to Settings  |
|                       |User Management               |  | 
|                       |Billing                       |
|                       |User Settings                 |  |                 | No change                 |
|                       |                              |  |                 | Git Personal Access Token |

### Account-level options

We moved all account-level options and settings to a single centralized location.  
You now have _single-click access_ to these options whenever you need:
* Account & user management
* Integrations
* Configurations
* Runtimes

Where:
* Click on the **Settings** (gear) icon on the toolbar.
SCREEN

{: .table .table-bordered .table-hover}
|  CLASSIC             |                               | |NEW              |           |
|  Bucket              | Menu item                     | |                 |             | 
| -------------------  | -------------------           |  | ----------------  | ---------     | 
| **GENERAL**          |  -                            |  | No change         |                          |       
|                      | **Account Information**       |  |                   | (Renamed) Organization Information |       
|                      | **Users & Teams**             |  |                   | Now in Access & Collaboration |
|                      | **Subscription & Billing**    |  |                   | No change                |
|                      | **Usage**                     |  |                   | No change                |
|                      |                               |  |                   |                          |       
| **CONFIGURATION**    |                               |  | No change         |                          |       
|                      | **Integrations**              |  |                   |(Renamed) Pipeline Integrations |       
|                      |                               |  |                   | GitOps Integrations (NEW)|       
|                      | **Pipeline Settings**         |  |                   | No change                |       
|                      | **Codefresh Runner**          |  |                   | (Moved) Runtimes bucket |
|                      | **Runtime Environments**      |  |                   | (Moved) Runtimes bucket |
|                      | **GitOps Controller**         |  |                   |  ??                       |
|                      | **Shared Configuration**      |  |                   | No change                |
|                      | **Nodes**                     |  |                   | ??                       |
|                      |                               |  |                   |                          |       
| **SECURITY**         |                               |  | (Renamed) Access & Collaboration|            |       
|                      | **Single Sign-On**            |  |                   | (Now in) Access & Collaboration |       
|                      | **Permissions**               |  |                   | Now in Access & Collaboration|       
|                      | **Audit**                     |  |                   | Now in Access & Collaboration  |       
|                      |                               |  |                   |                          |       
|                      |                               |  | Runtimes          |                                |       
|                      |                               |  |                   |  Build Runtimes (moved from Runtimes and renamed) |       
|                      |                               |  |                   |  GitOps Runtimes (NEW) |       



### Features and functionality
We renamed buckets and moved items for more intuitive access. 

{: .table .table-bordered .table-hover}
| Menu Bucket          | Menu                   | |                   |           |
| Classic              | Classic                 | |  New              |  New      | 
| -------------------  | -------------------     | |  -----------------| --------- | 
|                       |                        | |                  |  Home      |
|                       |                        | | Insights       |            |
|                       |                        | |               | DORA Metrics |
|**Build > Test > Deploy**|                      | |Pipelines      |
|                       |**Projects**            | |               | No change  |
|                       |**Pipelines**           | |               | No change  |
|                       |**Builds**              | |              | No change  |
|                       |**Steps**               | |               | No change  |
|                       |                        | |               |            |
|**DevOps Insights**    |                        | |Ops            |            |
|                       |                        | |                | GitOps Apps | 
|                       |**Kubernetes > Services** | |               | Kubernetes Services |
|                       |**Kubernetes > Monitor** | |               | ???
|                       |**Helm Releases**        | |               | No change  |
|                       |**Helm Boards**          | |               | No change  |
|                       |**Environments**         | |               | No change |
|                       |**Kubernetes > Monitor** | |               | No change
|                       |**GitOps**               | |               | Deprecated  |
|                       |                         | |               |            |
|**Artifacts**          |                         | | No change
|                       |**Images**               | |               |  No change |
|                       |**Helm Charts**          | |               |  No change |
|                       |**Compositions**         | |               |  No change |
|                       |                         | |               |            |
|**Settings**           |                         | |               |            |
|                       |**Account Settings**     | |               |(Moved) Settings in toolbar|
|                       |**User Settings**        | |               | Avatar     |






Server version 1.18 and higher, without Argo Project components. {::nomarkdown}<br><b>Tip</b>:  To check the server version, run:<br> <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl version --short</span>.{:/}