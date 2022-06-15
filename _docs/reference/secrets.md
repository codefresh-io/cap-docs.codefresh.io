---
title: "Secrets"
description: "Learn how Codefresh stores secrets"
group: reference
toc: true
---


Codefresh needs to store several types of secrets (credentials, keys for example), both for internal use and for external integrations with other systems.  

To accommodate secret storage, every Codefresh Runtime uses the [Bitnami Sealed Secrets controller](https://github.com/bitnami-labs/sealed-secrets) behind the scenes.
This controller is installed as part of the runtime and is automatically managed by Codefresh.

### How Sealed Secrets work

Sealed Secrets are based on [public/private key encryption](https://en.wikipedia.org/wiki/Public-key_cryptography). When the controller is installed, it gets a public and private key. The private key stays within the cluster. The public key can be given anywhere to encrypt secrets.
  
Any kind of secret can be encrypted with the public key (also via the `kubeseal` executable) and then passed to the cluster for decryption (once it is needed).

For GitOps applications, encryption for secrets is critical, as it means that you can commit any kind of secret in Git as long as it is encrypted.

Here's the event flow for Sealed Secrets:

1. A secret is encrypted by an operator and/or developer with the `kubeseal` executable.
1. The resulting file is a custom Kubernetes resource called SealedSecret.
1. The secret can now be committed in Git.
1. During application deployment, the Codefresh runtime applies this secret to the cluster.
1. The Sealed Secret controller identifies the Sealed Secret object and decrypts it using the private key of the cluster.
1. The Sealed Secret is converted to a [standard Kubernetes secret](https://kubernetes.io/docs/concepts/configuration/secret/) inside the cluster.
1. It is then passed to the application like any other secret (mounted file or environment variable).
1. The application uses the secret in its decrypted form.

For more details, you can read our [blog post for sealed secrets](https://codefresh.io/blog/handle-secrets-like-pro-using-gitops/).

### Configuring the Sealed Secrets controller

The Sealed Secret controller if fully managed by the Codefresh runtime. Secret encryption/decryption are fully automated.

> **Warning!** DO NOT tamper with the controller or its private/public keys in any way. 

The applications you deploy with Codefresh should also have no knowledge of the controller. All secrets that you need in your own applications should be accessed using the standard Kubernetes methods.

### What to read next  
[Install runtime]({{site.baseurl}}/docs/getting-started/quick-start/runtime)