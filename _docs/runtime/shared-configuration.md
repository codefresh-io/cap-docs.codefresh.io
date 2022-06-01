---
title: "Shared configuration for runtimes"
description: ""
group: runtime
toc: true
---

What is a shared configuration in Codefresh?

An account with multiple hosted runtimes or hybrid runtimes can create and store runtime configuration settings in a Git repository, and then share the configuration with any runtime that you choose. This means that you 

Codefresh creates the shared config repo for the first hosted and the first hybrid runtime that you install. Other runtimes use the same shared configuration repo for all subsequent runtimes that are installed. 

Hosted runtimes
As part of the set up for the hosted runtime, you must select the Git Organization for twhich the repors are to be created. Codefresh creates the repos.
Hybrid runtimes
Whn you install the first runtime on the cluster or for that account, you can define the shared configuration repo through the --shared-config-repo flag. If not defined, and the shated configuraiton repo for that runtime account does not exist, it is set to the shared-config root folder in the runtime installation repo.

Shared configuration directory strucure
Here is a sample shared configuraiton repo



```bash
├── resources <─────────────────┐
│   ├── all                     │
│   │   ├── manifest1.yaml      │
│   │   └── subfolder           │
│   │       └── manifest2.yaml  │
│   └── prod                    │
│       ├── manifest3.yaml      │
│       └── subfolder           │
│           └── manifest4.yaml  │
└── runtimes                    │
    ├── runtime1                │ # references by <install_repo_1>/apps/runtime1/config_dir.json
    │   ├── in-cluster.yaml    ─┤ #      manage `include` field to decide which directories to sync to cluster
    │   └── other-cluster.yaml ─┤ #      manage `include` field to decide which directories to sync to cluster
    └── runtime2                │ # references by <install_repo_2>/apps/runtime2/config_dir.json
        └── in-cluster.yaml    ─┘ #      manage `include` field to decide which directories to sync to cluster
```

The  base path of the repository includes two directories: `resources` and `runtimes`.

`resources` directory initially includes `all` tag directory, and then includes a sub-directory for each tag created.

`runtimes` directory includes a separate sub-directory for each runtime installed in the cluster. Each such runtime-directory will include in-cluster.yaml

Tags for 
When the user adds a new resource, an integration secret, for example, Codefresh sends the new “tag” name, a list of destination runtimes+clusters (no cluster defaults to in-cluster) and the required files' relative paths and string data. 

Application manifest for Shared Configuration repo

Every runtime has a Git-Source Application that targets the shared configuration repo in `runtimes/<runtime-name>`. And different Applications for every cluster managed by/registered to this runtime, entitled `isc-<cluster>`. These applications selectively syncs specific sub-directories in `resources` to the target cluster.

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
    repoURL: <account's-isc-repository>
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

Using “tags”
The main idea is similar to how RBAC is handled in k8s clusters.

Whenever you add a new resource, Codefresh creates a new "tag" name, a list of destination runtimes+clusters (no cluster defaults to in-cluster) and the required files' relative paths and string data.
The shared configurton's application for the shared reosu(in each runtime+cluster) will have its "tags" defined as part of its Include field (line #17 at the sample above).

When the user adds a new resource (an integration secret, for example), the UI will send a new “tag” name (probably the integration’s name), 
The ISC Application
Each runtime will have a Git-Source Application targeting the above ISC repo runtimes/<runtime-name> directory. This will create a separate isc-<cluster> application for each cluster this runtime is managing. These applications will selectively sync specific sub-directories of the resources directory to the target cluster.