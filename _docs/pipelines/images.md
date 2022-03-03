---
title: "Images"
description: ""
group: pipelines
toc: true
---

Building Docker images is one of the most basic requirements for creating Delivery Pipelines. 
Once you create an image, push the image to a registry, and report it to CSDP, image information is continually updated in the CSDP UI. 

### Bring image information to CSDP

1. Build the Docker image.
1. Push the image in any registry, or configure an artifact repository in CSDP to push images, as described in XREF.
1. Report image information to CSDP

#### Build and push a Docker image
Create a Docker image using our [Create a Docker image using Kaniko](https://codefresh.io/argohub/workflow-template/kaniko) Workflow Template.
Push the image to any registry.

#### Report image information to CSDP
[Codefresh Hub for Argo](https://codefresh.io/argohub/workflow-template/CSDP-metadata) offers a set of templates optimized to report and enrich Docker images. 

* Select the [report-image-info](https://github.com/codefresh-io/argo-hub/blob/main/workflows/codefresh-csdp/versions/0.0.6/docs/report-image-info.md) Workflow Template.


### Image views in CSDP 

Image views in CSDP are layered to show three levels of data: 
* Repository and application deployment
* Tags
* Summary

#### Filters for image views
As with any resource in CSDP, image views support filters that allow you focus on the data that's important to you.
Most image filters support multi-selection.  Unless otherwise indicated, the filters are common to all view levels.

{: .table .table-bordered .table-hover}
|  Filter          |  Description|  
| --------------   | --------------           |  
| **Repository Names** | The Git repository or repositories that contain the image.  |                            
| **Tag**              | The tag by which to filter. |  
| **Registry Types**   | The registry which stores your image. To filter by registries that are not listed, select **Other types**.|
| **Git branch**       | The Git branch to which the image is pushed.|
| **Git repositories** | The Git provider you use.|      
| **Deployed in application**| The application or applications in which the image is currently deployed.|
| **Sorted by** | List images by **Name**, or by the most recent update, **Last update**.



#### Image repository and deployment view
The default view for image resources shows repository and deployment information.

{% include 
   image.html 
   lightbox="true" 
   file="/images/image/images-application-level.png" 
   url="/images/image/images-application-level.png" 
   alt="Deployment info for Images in CSDP" 
   caption="Deployment info for Images in CSDP"
   max-width="30%" 
   %}

{: .table .table-bordered .table-hover}
|  Legend          |  Description|  
| --------------   | --------------           |  
| **1**            | The name of the image.   |                            
| **2**            | The applications in which the image is currently deployed. Select the application to go to the Applications dashboard.|  
| **3**            | The details on the most recent commit associated with the image. Select the commit to view the changes in the Git repository.|
| **4**            | Binary information on the image.|
| **5**            | The tag associated with the image in the application. |       
| **6**            | The registry to which the image is pushed, and from which it is distributed.|
                     
### Image tag view
Drilldown on the repository shows tag information for the image.

{% include 
   image.html 
   lightbox="true" 
   file="/images/image/images-tag-view.png" 
   url="/images/image/images-tag-view.png" 
   alt="Tag info for Images in CSDP" 
   caption="Tag info for Images in CSDP"
   max-width="30%" 
   %}

{: .table .table-bordered .table-hover}
|  Legend          |  Description|  
| --------------   | --------------           |  
| **1**                | The image tag.   |                            
| **2**                | The comment describing the commit or change, with the name of the Git provider and the corresponding PR. To view details of the commit changes in the Git repository, select the commit text.|  
| **3**                | The hash of the Docker image, generated as sha256. A change in the digest indicates that something has changed in the image.|
| **4**                | The registry to which the image is pushed (stored), and from which it is distributed.|
| **5**                | The OS and architecture in which the image was created. |       
| **6**                | The date and time of the most recent update in the local time zone. |
| **7**                | The size of the image.
| **8**               | Additional information on the image. To view the Summary, select **more details**.

###  Image summary view
The Summary view shows metadata for the image. 
Selecting **more details** for an image tag.

{% include 
   image.html 
   lightbox="true" 
   file="/images/image/images-summary-view.png" 
   url="/images/image/images-summary-view.png" 
   alt="Summary info for Images in CSDP" 
   caption="Summary info for Images in CSDP"
   max-width="30%" 
   %}

{: .table .table-bordered .table-hover}
|  Legend          |  Description|  
| --------------   | --------------           |  
| **1**            | The bugs or fix requests opened on this image tag.    |                            
| **2**            | The pull request or requests pending comment.|  
| **3**            | The annotations created for the image if any.|
| **4**            | The applications in which the tagged version of the image is deployed.|
| **5**            | The Git details for this image tag, such as the Git hash, the Jira issue number, Git Pull Request, commit information, the name of the user who performed the commit. |       
| **6**            | Select to view the log information for the build image step in the relevant workflow.|



