---
title: "Viewing Images in Codefresh"
description: ""
group: pipelines
toc: true
---

Building Docker images is one of the most basic requirements for creating Delivery Pipelines. 
Once you create an image, push the image to a registry, and report it to Codefresh, image information is continually updated in the Codefresh UI. 

### Requirements for images in Codefresh
Complete the mandatory steps to see your images in Codefresh. Each step has links to examples in the Codefresh Hub for Argo.  

1. (Mandatory) Build the Docker image, and push the image to any registry.  
  See [Create a Docker image using Kaniko](https://codefresh.io/argohub/workflow-template/kaniko){:target="\_blank"}.
1. (Optional) Enrich image information with annotations and metadata.  
  For Git and Jira image enrichment examples, see [Codefresh-metadata image enrichment](https://codefresh.io/argohub/workflow-template/CSDP-metadata){:target="\_blank"}.
1. (Mandatory) Report image information to Codefresh.  
  See the [report-image-info](https://github.com/codefresh-io/argo-hub/blob/main/workflows/codefresh-csdp/versions/0.0.6/docs/report-image-info.md){:target="\_blank"} example.
  

### Image views in Codefresh 
* In the Codefresh UI, go to [Images](https://g.codefresh.io/2.0/images){:target="\_blank"}.

Image views are layered to show three levels of data: 
* Repository and application deployment
* Tags
* Summary with metadata and binary information 

#### Filters for image views
As with any resource in Codefresh, image views support filters that allow you focus on the data that's important to you.
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
   file="/images/image/application-level.png" 
   url="/images/image/application-level.png" 
   alt="Repository & deployment info for Images in Codefresh" 
   caption="Repository & deployment info for Images in Codefresh"
   max-width="30%" 
   %}

{: .table .table-bordered .table-hover}
|  Legend          |  Description|  
| --------------   | --------------           |  
| **1**            | The name of the image.   |                            
| **2**            | The applications in which the image is currently deployed. Select the application to go to the Applications dashboard.|  
| **3**            | The details on the most recent commit associated with the image. Select the commit to view the changes in the Git repository.|
| **4**            | Binary information on the image.|
| **5**            | The registry to which the image is pushed, and from which it is distributed.|
                     
### Image tag view
Drilldown on the repository shows tag information for the image.
{% include 
   image.html 
   lightbox="true" 
   file="/images/image/tag-view.png" 
   url="/images/image/tag-view.png" 
   alt="Tag info for Images in Codefresh" 
   caption="Tag info for Images in Codefresh"
   max-width="30%" 
   %}

{: .table .table-bordered .table-hover}
|  Legend          |  Description|  
| --------------   | --------------           |  
| **1**                | The image tag.   |                            
| **2**                | The comment describing the commit or change, with the name of the Git provider and the corresponding PR. To view details of the commit changes in the Git repository, select the commit text.|  
| **3**                | The hash of the Docker image, generated as sha256. A change in the digest indicates that something has changed in the image.|
| **4**                | The registry to which the image is pushed (stored), and from which it is distributed.|
| **5**                | The OS and architecture in which the image was created. The date and time of the most recent update is in the local time zone|       
| **6**                | Additional information on the image. To view the Summary, select **more details**.|

###  Image summary view
The Summary view shows metadata for the image. 
Select **more details** for an image tag to go to the Image summary.

{% include 
   image.html 
   lightbox="true" 
   file="/images/image/image-summary-tab.png" 
   url="/images/image/image-summary-tab.png" 
   alt="Summary info for Images in Codefresh" 
   caption="Summary info for Images in Codefresh"
   max-width="30%" 
   %}

{: .table .table-bordered .table-hover}
|  Legend          |  Description|  
| --------------   | --------------           |  
| **1**            | The image that is currently deployed and the number of applications it is deployed in.|   
| **2**            | The metadata for the image, including the link to the version of the image currently deployed.|  
| **3**            | The Jira issues if any fixed in this version.|  
| **4**            | The Git repo information for this image tag, including PR (Pull Request) and commit information. |  
| **5**            | The application or the list of applications in which this image is deployed, with cluster and namespace information for each.| 
| **6**            | The size of the image and the link to the workflow that generated the image step. The external workflow URL is reported only when `WORKFLOW_URL` is used as input. See [report-image-info](https://github.com/codefresh-io/argo-hub/blob/main/workflows/codefresh-csdp/versions/0.0.6/docs/report-image-info.md).|
| **5**             | The link to the log for the build image step in the relevant workflow. Similar to the external workflow URL, the external log URL is also reported when `LOGS_URL` is used as input. |
