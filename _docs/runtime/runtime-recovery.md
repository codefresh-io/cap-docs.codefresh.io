---
title: "Recover runtimes"
description: ""
group: runtime
toc: true
---

In case of cluster failure, you can recover the runtime installed in the cluster from the repository with the resources.
Runtime recovery is possible for partial or complete cluster failures. You can reinstall the runtime in the same or a different cluster.

The installation process:
* Applies `argo-cd` from the installation manifests in your repo to your cluster
* Associates `argo-cd` with the existing repo (what does this mean? is this the repo URL?).
* Applies the runtime and `argo-cd` secrets to the cluster
* Updates the runtime config map (`<runtime-name>.yaml` in the `bootstrap` directory) with the new cluster configuration  

All other fields such as `cluster`, `ingressClassName`, `ingressController` and `ingressHost` remain unchanged.  

### Before you begin

* During reinstallation, in the same or a different cluster, you will need to provide the following information: 
  > The first three values must be the same as the runtime you are trying to recover. 
  * Runtime name
  * Repository URL
  * Codefresh context
  * Kube context: Only if you are installing on the same cluster

* Make sure you have a registered Git integration.  
  To create one, use `cf integration git add default --runtime <runtime-name> --provider github --api-url https://api.github.com` 


### How to recover a runtime
Reinstall the runtime from an existing repository to recover it, in the same or a different cluster.  


1. Run:  
  `cf runtime install --from-repo`.
1. Provide the relevant values when prompted.
1. If you are performing runtime recovery in a different cluster, you may need to reconfigure the ingress resources for `app-proxy`, `workflows` and `default-git-source`.  
  If the health status remains as `Progressing`, do the following:
  
    * In the runtime installation repo, verify that the `ingress.yaml` files for the `app-proxy` and `workflows` are configured with the correct `host` and `ingressClassName`:  

      `apps/app-proxy/overlays/<runtime-name>/ingress.yaml`  
      `apps/workflows/overlays/<runtime-name>/ingress.yaml`  

    * Go to the Git Source repository, and verify the `host` and `ingressClassName` for `cdp-default-git-source.ingress.yaml`: 

       `resources_<runtime-name>/cdp-default-git-source.ingress.yaml`  
    
  See the [example](#ingress-example) below. 

1. If you have managed clusters registered to the runtime you are recovering, reconnect them.  
  Run the command and follow the instructions in the wizard:  
  `cf cluster add`

1. Verify that you have a registered Git integration:  
  `cf integration git list --runtime <runtime-name>`  
    

### Ingress example
This is an example of the `ingress.yaml` for `workflows`.

 ```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    ingress.kubernetes.io/protocol: https
    ingress.kubernetes.io/rewrite-target: /$2
    nginx.ingress.kubernetes.io/backend-protocol: https
    nginx.ingress.kubernetes.io/rewrite-target: /$2
  creationTimestamp: null
  name: runtime-name-workflows-ingress
  namespace: runtime-name
spec:
  ingressClassName: nginx
  rules:
  - host: your-ingress-host.com
    http:
      paths:
      - backend:
          service:
            name: argo-server
            port:
              number: 2746
        path: /workflows(/|$)(.*)
        pathType: ImplementationSpecific
status:
  loadBalancer: {}
```


