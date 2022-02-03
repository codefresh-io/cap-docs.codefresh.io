---
title: "Configure atrifact repository"
description: ""
group: pipelines
toc: true
---

Configure an artifact repository to run Argo workflows that use artifacts. Configure any Argo-supported S3-compatible repository such as AWS, GCS and Minio.  

If you enable automatic logging for pipelines, pipeline log artifacts are also stored in this repo.

### Define artifact repository 

1. Go to the folder: `<runtime_installation_repo>/apps/workflows/overlays/codefresh-v2-production`.
1. Either create a new file, or change the settings in the default `artifact-repo.yaml` shown below.
1. In the data section:
  * To enable archiving for pipeline logs, set `archiveLogs` to `true`.
  * Below `s3`, add the required values:
    `bucket`, for example, `codefresh-v2-production-artifacts`  
    `endpoint`, for example, `s3.amazonaws.com`  
    `region`, for example, `Leaus-east-1`  
    `useSDKCreds` as `true`

  ```yaml
  apiVersion: v1
  kind: ConfigMap
  metadata:
    annotations:
      workflows.argoproj.io/default-artifact-repository: default-v1
    name: artifact-repositories
  data:
    default-v1: |
      archiveLogs: true
      s3:
        bucket: <s3-bucket-name> 
        endpoint: s3.amazonaws.com
        region: us-east-1
        useSDKCreds: true
  ```

### Define RBAC permissions for S3 bucket
Grant the workflow controller sufficient permissions for S3 bucket operations.

1. Go to the same folder: `runtime_installation_repo>/apps/workflows/overlays/codefresh-v2-production`.
1. Open the `rbac.yaml` file.

