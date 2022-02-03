---
title: "Configure atrifact repository"
description: ""
group: pipelines
toc: true
---

To run Argo Workflows that use artifacts, configure an artifact repository. Configure any Argo-supported, S3-compatible repository, such as AWS, GCS or Minio.    
Artifact repos are also required to archive pipeline logs when automatic logging is enabled for pipelines.  

To configure an artifact repository:  

Create a `ConfigMap` with the specifications   
Configure `RBAC` permissions for the workflow controller
Update the `serviceAccountName` to match the storage

### Create ConfigMap for artifact repository 
Create a `ConfigMap` with the specs to connect to the storage bucket configured as the artifact repository, and enable pipeline logging to the same. The settings apply to all workflows by default, unless overridden by a specific workflow template or workflow.

1. Go to your CSDP runtime installation repository:  
   `<runtime_installation_repo>/apps/workflows/overlays/<runtime-name>/`  
1. Create a new file entitled `artifact-repo.yaml`,and update `bucket`,`endpoint`, and `region`:  
```yaml
---
apiVersion: v1
kind: ConfigMap
metadata:
  annotations:
    workflows.argoproj.io/default-artifact-repository: default-v1
  name: artifact-repositories
data:
  default-v1: |
    archiveLogs: true # enable pipeline logging
    s3:
      bucket: <s3-storage-bucket-name> # for example, `codefresh-v2-production-artifacts`
      endpoint: <endpoint> # for example, `s3.amazonaws.com`
      region: <region> # for example, `us-east-1`
      useSDKCreds: true
```

### Define RBAC permissions for artifact repository
Grant the workflow controller sufficient permissions for S3 bucket operations.

1. Go to the same CSDP runtime installation repository:  
  `<runtime_installation_repo>/apps/workflows/overlays/<runtime-name>`  
1. Create the `rbac.yaml` file with the permissions, changing the annotation to match the S3 storage :  
```yaml 
apiVersion: v1
kind: ServiceAccount
metadata:
  name: workflow
  annotations: # change below as needed 
    <endpoint>/role-arn: <role-arn> # example s3.amazonaws.com/role-arn = arn:aws:iam::559963890471:role/argo-workflows-s3-artifact-repo
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: workflow-role
rules:
- apiGroups:
  - ""
  resources:
  - pods
  verbs:
  - get
  - watch
  - patch
- apiGroups:
  - ""
  resources:
  - pods/log
  verbs:
  - get
  - watch
- apiGroups:
  - ""
  resources:
  - pods/exec
  verbs:
  - create
- apiGroups:
  - ""
  resources:
  - configmaps
  verbs:
  - create
  - get
  - update
- apiGroups:
  - argoproj.io
  resources:
  - workflows
  - workflowtemplates
  verbs:
  - create
  - delete
  - list
  - get
  - watch
  - update
  - patch
- apiGroups:
  - argoproj.io
  resources:
  - workflowtasksets
  - workflowtasksets/finalizers
  verbs:
  - list
  - watch
  - get
  - update
  - patch
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: workflow-default-binding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: workflow-role
subjects:
- kind: ServiceAccount
  name: workflow
---
```
  

### Update serviceAccountName for artifact repository
Define the correct Service Account that with the roles and permissions for workflows to access the artifact repository.  
{:start="1"}
1. Go to the same CSDP runtime installation repository:  
  `<runtime_installation_repo>/apps/workflows/overlays/<runtime-name>`  
1. Open `kustomization.yaml`.
1. Change the `serviceAccountName`:
```yaml
data:
  workflowDefaults: |
    spec:
      serviceAccountName: <service-account-name> # IMPORTANT! Provides access to S3 artifact repo              
```