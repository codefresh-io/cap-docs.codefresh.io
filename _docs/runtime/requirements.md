---
title: "Draft: System requirements"
description: ""
group: runtime
toc: true
---


The requirements listed are the **_minimum_** requirements for CSDP (Codefresh Software Developement Platform).

### Memory and CPU

Memory: 3000 MB  
CPU: 2

### RBAC permissions for resources

{: .table .table-bordered .table-hover}
|  Resource                   |  Permissions Required|  
| --------------            | --------------           |  
| `ServiceAccount`            | Create, Delete         |                             
| `ConfigMap`                 | Create, Update, Delete |          
| `Service`                   | Create, Update, Delete |       
| `Role`                       | In group `rbac.authorization.k8s.io`: Create, Update, Delete |       
| `RoleBinding`               | In group `rbac.authorization.k8s.io`: Create, Update, Delete  | 
| `persistentvolumeclaims`    | Create, Update, Delete               |   
| `pods`                       | Creat, Update, Delete               | 

### What to read next
[Runtime installation]({{site.baseurl}}/docs/runtime/requirements/)