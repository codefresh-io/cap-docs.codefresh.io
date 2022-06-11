---
title: "Shared configuration for runtimes"
description: ""
group: runtime
toc: true
---


A Codefresh account with a hosted or a hybrid runtime can create a Git repository in which to store configuration settings for the runtime. This repository can then be shared with subsequent runtimes that are installed. For the first hosted or hybrid runtime that you install, Codefresh creates the shared config repo. You can then selectivley select the shared repository for For every runtime install, you can select the runtimes that use this shared configuration. 

* Hosted runtimes
  As part of the set up for the hosted runtime, you must select the Git Organization for which to create the runtime installation repo. Codefresh creates the repo for the shared runtime configuration.  

* Hybrid runtimes 
  When you install the first runtime on the cluster or for that account, you can define the shared configuration repo through the `--shared-config-repo` flag. If the flag is omitted, and the runtime account does not have a shared configuration repo, it is set to the `shared-config` root folder in the runtime installation repo.

>Currently, you can have a single shared configuration repo per account.


### Shared configuration repo directory structure
Here is an example of the shared configuration repository structure. 

```bash
.
├── resources <───────────────────┐
│   ├── all-runtimes-all-clusters │
│   │   ├── manifest1.yaml        │
│   │   └── subfolder             │
│   │       └── manifest2.yaml    │
│   ├── control-planes            │
│   │   └── manifest3.yaml        │
│   ├── runtimes                  │
│   │   ├── runtime1              │
│   │   │   └── manifest4.yaml    │
│   │   └── runtime2              │
│   │       └── manifest5.yaml    │
│   └── manifest6.yaml            │
└── runtimes                      │
    ├── runtime1                  │ # referenced by <install_repo_1>/apps/runtime1/config_dir.json
    │   ├── in-cluster.yaml      ─┤ #     manage `include` field to decide which dirs/files to sync to cluster
    │   └── other-cluster.yaml   ─┤ #     manage `include` field to decide which dirs/files to sync to cluster
    └── runtime2                  │ # referenced by <install_repo_2>/apps/runtime2/config_dir.json
        └── in-cluster.yaml      ─┘ #     manage `include` field to decide which dirs/files to sync to cluster
```

#### `resources` directory 

The resources directory holds the resources shared by all clusters managed by the runtime, in-cluster specific respuirces, runtime-specific resources.

  * `all-runtimes-all-clusters`: As the name implies, every manifest in this directory is applied to all the clusters managed by this runtime. 
  * `control-plane`: Optional, for hosted runtimes only. When defined, every manifest in this directory is applied to each hosted runtime’s `in-cluster`.
  * `runtimes/<runtime_name>`: Optional. Sub-directory specific to each runtime, `runtime1` in the above example. Every manifest in a runtime-specific sub-directory is applied to all clusters managed by that runtime.

#### `runtimes` directory 
A sub-directory specific to each runtime installed in the cluster, including its `in-cluster.yaml` In the above example, there are two sub-directories for `runtime1` and `runtime2`, each with its own `in-cluster.yaml`.

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  labels:
    codefresh.io/entity: internal-config
    codefresh.io/internal: 'true'
  name: isc-in-cluster
spec:
  destination:
    namespace: isc
    server: https://kubernetes.default.svc
  project: default
  source:
    repoURL: <account's-shared-config-repository>
    path: resources # or shared-config/resources
    directory:
      include: '{all/*.yaml,all/**/*.yaml}' # order and structure is imporant
      recurse: true
  syncPolicy:
    automated:
      allowEmpty: true
      prune: true
      selfHeal: true
    syncOptions:
      - allowEmpty=true
```

#### Adding resources
When creating a new resource such as a new integration for example, you must define the runtimes and clusters to which to apply that resource. The app-proxy saves the resource in the correct location, and updates the relevant Argo CD Applications to include it.

#### Application manifest for Shared Configuration repo

Every runtime has a Git-Source Application that targets the shared configuration repo in `runtimes/<runtime-name>`. The Git Source application in turn creates a separate `isc-<cluster>` application for every cluster managed by the runtime. These applications selectively sync specific sub-directories of the resources directory to the target cluster.

### Upgrading older runtimes

For older runtime versions without the shared configuration repository, you must upgrade the runtimes. There are two options:
* Upgrade the runtime, and let the app-proxy create the shared configuration repo automatically. 
* Manually define the shared configuration repository, by adding the `--shared-config-repo` flag in the runtime upgrade command. 

If the runtime being upgraded has managed clusters, once the shared configuration repo is created for the account, either automatically or manually on upgrade, all clusters are migrated to the same repo when app-proxy is initialized. An argoproj application manifest is committed to the  repo for each cluster managed by the runtime. 


