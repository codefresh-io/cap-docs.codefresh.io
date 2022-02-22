---
title: "Images"
description: ""
group: pipelines
toc: true
---

Building a Docker image and then pushing it to a registry is one of the most basic scenarios for creating a delivery pipeline. 
Once you create an image and deploy it, and either report it to CSDP or add it to the artifact repository configured, and it is deployed  ( like staging, prod, etc ) codefresh identify it and adds such information to the images view  
you can see image data 

Once you build an image you can store in one 

### Build and report a Docker image
To create a Docker image, we have a ready-to-use Workflow Template, [Create a Docker image using Kaniko](https://codefresh.io/argohub/workflow-template/kaniko).

#### Report image information to CSDP
[Codefresh Hub for Argo](https://codefresh.io/argohub/workflow-template/CSDP-metadata) offers a set of templates optimized to report and enrich Docker images. 

* Select the [report-image-info](https://github.com/codefresh-io/argo-hub/blob/main/workflows/codefresh-csdp/versions/0.0.6/docs/report-image-info.md) template.



### Filters for image views
Image views in CSDP are layered in a top-down down model, from high-level deployment to binary information.  

As with any resource in CSDP, Image views support filters that allow you focus on the data that's important to you.
Most image filters support multi-select.  Unless otherwise indicated, the filters are common to all image views

{: .table .table-bordered .table-hover}
|  Filter          |  Description|  
| --------------   | --------------           |  
| Repository Names | The Git repository or repositories that contain the image.  |                            
| Tag              | The tag by which to filter. |  
| Registry Types   | The Docker registry which stores your image. To filter by registries that are not listed, select **Other types**.|
| Git branch       | The Git branch to which the image is pushed (stored).|
| Git repositories | ???|       
| Deployed in application| The application in which the image is currently deployed.|
| Sorted by | List images by **Name** or the most recent update **Last update**.



### Image deployment view
The default image view shows deployment information for the image.

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
| 1                | The name of the image.   |                            
| 2                | The applications in which the image is currently deployed. Select the application to go to the Applications dashboard.|  
| 3                | The details on the most recent commit associated with the image. Select the commit to view the changes in the Git repository.|
| 4                | Binary information on the image.|
| 5                | The tag associated with the image in the application. |       
| 6                | The registry to which the image is pushed (stored), and from which it is distributed.|
                     
### Image tag view
Drilldown on the image name shows the tag information for the image.

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
| 1                | The image tag.   |                            
| 2                | The comment describing the commit or change, with the name of the Git provider and the corresponding PR. To view details of the commit changes in the Git repository, select the commit text.|  
| 3                | The hash of the Docker image, generated as sha256. A change in the digest indicates that something has changed in the image.|
| 4                | The registry to which the image is pushed (stored), and from which it is distributed.|
| 5                | The OS and architecture in which the image was created. |       
| 6                | The date and time of the most recent update, in UTC. |
| 7                | The size of the image.
| 8                | The final layer of image information, summarizing the information in the application and tag layers. To view the Summary, select **more details**.

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
| 1                | The bugs or fix requests opened on this image tag.    |                            
| 2                | The pull request or requests pending comment.|  
| 3                | The annotations created for the image if any.|
| 4                | The applications in which the tagged version of the image is deployed.|
| 5                | The Git details of this image tag, such as the Git pprovider, the Pull Request, the username  |       
| 6                | Image logs|

