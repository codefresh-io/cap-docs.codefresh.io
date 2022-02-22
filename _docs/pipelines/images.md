---
title: "Images"
description: ""
group: pipelines
toc: true
---

Building a Docker image and then pushing it to a registry is one of the most basic scenarios for creating a a delivery pipeline. 
Once you build and report your image to CSDP, CSDP displays imaage data as pne of the resources. 
you can see image data 

Once you build an image you can store in one 

### Build and report a Docker image
To create a Docker image, we have a ready-to-use a Workflow Template. [Create a Docker image using Kaniko](https://codefresh.io/argohub/workflow-template/kaniko).

#### Report image information to CSDP
[Codefresh Hub for Argo](https://codefresh.io/argohub/workflow-template/CSDP-metadata) offers a set of templates optimized to report and enrich Docker images. 

* Select the [report-image-info](https://github.com/codefresh-io/argo-hub/blob/main/workflows/codefresh-csdp/versions/0.0.6/docs/report-image-info.md) template.

### Images in CSDP

The 

Once you create an image and eihter report it to CSDP or add it to the artifact repository configured, and it is deployed  ( like staging, prod, etc ) codefresh identify it and adds such information to the images view 

The inital view is un filtered view of all the images deployed in applications. 
For each image you see the list of applications, the last commit, date it was created, image size, and the image tag. At the bottom is the registry you image is pushed to. 

 Codefresh enables you to integrate with several Docker container registries, including

Application-level information
The default image view in CSDP shows deployment information for the image.

{% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/quick-start/quick-start-ci-pipeline-arguments.png" 
   url="/images/getting-started/quick-start/quick-start-ci-pipeline-arguments.png" 
   alt="Predefined variables for arguments" 
   caption="Predefined variables for arguments"
   max-width="30%" 
   %}

{: .table .table-bordered .table-hover}
|  Legend          |  Description|  
| --------------   | --------------           |  
| 1                | The name of the image.   |                            
| 2                | The applications in which the image is deployed.|          
| 3                | The tag associated with the image in the application.|
| 4                | Binary information on the image. |       
| 5                | The registry to which the image is pushed (stored), and from which it is distributed.|
                     