---
title: "Main concepts"
description: ""
group: getting-started
toc: true
---

## Built on top of the open source Argo
CSDP maintains a [conformed](https://github.com/argoproj/argo-conformance-program) fork of the following Argo components and provides an enterprise supported version of them:
* [Argo CD](https://github.com/argoproj/argo-cd) - Declarative continuous deployment for Kubernetes.
* [Argo Rollouts](https://argoproj.github.io/argo-rollouts/) - Progressive Delivery for Kubernetes.
* [Argo Workflows](https://github.com/argoproj/argo-workflows) - Workflow engine for Kubernetes.
* [Argo Events](https://github.com/argoproj/argo-events) - Event-driven workflow automation framework.

Checkout the full [architecture documentation](architecture.md) of CSDP.

## Hybrid behind firewall model
CSDP perform an installation which is called a Runtime on the user's k8s cluster which contains all required components for making the entire experience to work. <br>
Checkout the full [architecture documentation](./architecture.md) for more details.

## GitOps native approach
CSDP is built entirely on the heavily adopted concept of GitOps. <br>
Checkout a detailed explanation of [GitOps](./gitops.md). <br>
CSDP leverages Argo components (Argo CD and Events) in order to have the entire desired state applied from Git to the user's k8s cluster and also reported back to CSDP platform. 

## Every state change operation in CSDP is made via Git
CSDP has taken the GitOps approach a step forward by making the entire entity model fully controlled by GitOps via the CSDP [GitSource entity](entity-model.md), which means the entire state of your account will be maintained in Git.
CSDP provides a full frontend experience powered by a strong API layer (GraphQL), and every state change (via GraphQL mutation) will actually perform a commit on behalf of the user to Git.

## Audit log derived from Git changelog
CSDP has built its sophisticated but simple audit log on all operations to the system on both the git change and the log of api calls that have been made to the system.
Checkout the full [auditing documentation](../administration/audit.md)

## Access control derived from Git permissions
CSDP has built its sophisticated but simple access control model on top of the existing Git operations that are defined externally to the system. <br>
Checkout the full [access control documentation](../administration/access-control.md)
