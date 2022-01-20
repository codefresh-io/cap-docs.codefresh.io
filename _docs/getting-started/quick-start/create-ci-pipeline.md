---
title: "Create a basic CI pipeline"
description: ""
group: getting-started
sub-group: quick-start
toc: true
---

Now that you have configured and run the Hello World demo pipeline, let's create a more advanced pipeline.  

For the quick start, you will create a basic CI delivery pipeline, in the CSDP, and via Git.  

The delivery pipeline:  
* Clones a Git repository
* Builds a docker image using `kaniko`
* Pushes the built image to a Docker Registry
* Runs an example testing step
* Sends the image information to CSDP

Our CI pipeline interacts with third-party services such as GitHub and a Docker Registry. We need to first add secrets to the cluster to store the credentials required. These tasks are common to both methods of creating pipelines.  


#### Create a Personal Access Token (PAT)
You must have a PAT to clone the repository. 

1. Create your personal token with a valid `expiration` date and `scope` with `base64` encoding.  
  For CSDP pipelines, you need `repo` and `admin-repo.hook` scopes:  
  
  {% include 
   image.html 
   lightbox="true" 
   file="/images/getting-started/quick-start/quick-start-git-event-permissions.png" 
   url="/images/getting-started/quick-start/quick-start-git-event-permissions.png" 
   alt="GitHub PAT permissions for CI pipeline" 
   caption="GitHub PAT permissions for CI pipeline"
   max-width="30%" 
   %}  

{:start="2"}
1. Download [`github-token.secret.example.yaml`](https://github.com/eti-codefresh/quickstart_resources/blob/add491550d4a652fc62780173ce4fc9bfba24e58/github-token.secret.example.yaml).  
1. Replace the placeholder for the `token` field value with your PAT token.

  ```yaml
    apiVersion: v1
    kind: Secret
    metadata:
      name: github-access
    type: Opaque
    data:
      token: <paste-your-pat-token-here> 
  ```
{:start="3"}
1. Save and apply the file to your cluster in the namespace created when you installed the CSDP Runtime:  

   `kubectl apply -n <csdp-runtime-namespace> -f github-token.secret.example.yaml`  
    where:  
      `<csdp-runtime-namespace>` is the namespace created during runtime installation.


#### Create Docker-registry secret

To push the image to a Docker registry, create a secret to use with Docker registry.  

1. Create the Docker-registry secret by following the instructions in this [link](​​https://jamesdefabia.github.io/docs/user-guide/kubectl/kubectl_create_secret_docker-registry/).
1. Create the secret for our `image-reporter` step:  

  * Create a secret resource.
  * Download [`registry-creds.secret.example.yaml`](https://github.com/eti-codefresh/quickstart_resources/blob/add491550d4a652fc62780173ce4fc9bfba24e58/registry-creds.secret.example.yaml).
  * Replace the `username`, `password` and `domain` with your `base64` encoded credentials.
1. Save and apply the file to your cluster in the namespace created when you installed the CSDP Runtime:  

   `kubectl apply -n <csdp-runtime-namespace> -f registry-creds.secret.example.yaml`  
    where:  
      `<csdp-runtime-namespace>` is the namespace created during runtime installation.


### Continue with
[CSDP: Create a CI delivery pipeline]({{site.baseurl}}/docs/getting-started/quick-start/ci-pipeline/csdp-create-ci-pipeline)  
[Git: Create a CI delivery pipeline]({{site.baseurl}}/docs/getting-started/quick-start/ci-pipeline/git-create-ci-pipeline)  


