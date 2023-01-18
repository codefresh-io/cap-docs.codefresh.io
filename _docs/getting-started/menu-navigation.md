---
title: ""
description: ""
group: getting-started
toc: true
---

## Classic to New navigation
Get up to speed with the changes in the navigation.

We added a new **Settings** icon to the toolbar to simplify access to account-level options, typically handled by Codefresh administrators.
All account-level options and settings are in the same location. You now have _single-click access_ to these options whenever you need.



{% include 
  image.html 
  lightbox="true" 
  file="/images/getting-started/settings-icon.png" 
  url="/images/getting-started/settings-icon.png" 
  alt="Settings icon in toolbar" 
  caption="Settings icon in toolbar"
    max-width="70%" 
%}

### Avatar options
|  CLASSIC             |                               |  |NEW              |             |
|                      | Menu item                     |  |                 |             | 
| -------------------  | -------------------           |  | ---------------- | ---------  | 
|Avatar                 |                              |  |                  |            |     
|                       |Account Settings              |  |                  | Moved to Settings  |
|                       |User Management               |  |                  | Moved to Settings  |               
|                       |Billing                       |  |                  | Moved to Settings  |             
|                       |User Settings                 |  |                  | No change                 |
|                       |                              |  |                  | Git Personal Access Token |

### Account-level options

Access account-level options through the Settings icon: 
* Account & user management
* Integrations
* Configurations
* Runtimes



{: .table .table-bordered .table-hover}
|  CLASSIC             |                               |&rarr; |NEW           |           |
|  Bucket              | Menu item                     | |                    |             | 
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
|  CLASSIC             |                         | |&rarr;              |NEW          |    
|  Bucket              | Menu item               | |                    |            | 
| -------------------  | -------------------     |  | ----------------  | ---------  | 
|                       |                        | |                    |  Home      |
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


