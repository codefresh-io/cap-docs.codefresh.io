---
title: "CI/CD pipeline examples"
description: "A collection of examples for Codefresh pipelines"
group: example-catalog
redirect_from:
  - /docs/examples-v01/
  - examples.html
  - /docs/catalog-examples/
  - /docs/examples/
  - /docs/codefresh-yaml-examples/  
  - /docs/codefresh-yaml/codefresh-yaml-examples/
toc: true
---
Codefresh enables you to define the steps of your pipeline in a [YAML file]({{site.baseurl}}/docs/pipelines/what-is-the-codefresh-yaml/). By default, the file is named `codefresh.yml`, and is located in the root directory of the repository.

## Programming-language specific examples

Codefresh is agnostic as far as programming languages are concerned. All major programming languages are supported:

- [Go Web App]({{site.baseurl}}/docs/example-catalog/golang/golang-hello-world/) or [Go CLI]({{site.baseurl}}/docs/example-catalog/golang/goreleaser) 
- [Spring Java app with Maven]({{site.baseurl}}/docs/example-catalog/java/spring-boot-2/) or [Gradle]({{site.baseurl}}/docs/example-catalog/java/gradle/). Also how to [upload JAR to Nexus/Artifactory]({{site.baseurl}}/docs/example-catalog/java/publish-jar/) 
- Node [Express.js App]({{site.baseurl}}/docs/example-catalog/nodejs/lets-chat/) or [React.js App]({{site.baseurl}}/docs/example-catalog/nodejs/react/)
- [Php App]({{site.baseurl}}/docs/example-catalog/php)
- [Python Django App]({{site.baseurl}}/docs/example-catalog/python/django/)
- [Ruby On Rails App]({{site.baseurl}}/docs/example-catalog/ruby/)
- [C]({{site.baseurl}}/docs/example-catalog/cc/c-make/) or [C++]({{site.baseurl}}/docs/example-catalog/cc/cpp-cmake)
- [Rust]({{site.baseurl}}/docs/example-catalog/rust/) 
- [C# .NET core]({{site.baseurl}}/docs/example-catalog/dotnet/)
- [Scala App]({{site.baseurl}}/docs/example-catalog/scala/scala-hello-world/)
- [Android (Mobile)]({{site.baseurl}}/docs/example-catalog/mobile/android/)

## Source code checkout examples

You can check out code from one or more repositories in any pipeline phase. Codefresh includes [built-in GIT integration]({{site.baseurl}}/docs/integrations/git-providers/) with all the popular GIT providers and can be used with [git-clone]({{site.baseurl}}/docs/codefresh-yaml/steps/git-clone/) steps.

- [Cloning Git repositories using the built-in integration]({{site.baseurl}}/docs/example-catalog/git-checkout/)
- [Cloning Git repositories using manual Git commands]({{site.baseurl}}/docs/example-catalog/git-checkout-custom/)
- [Checking out from Subversion, Perforce, Mercurial, etc ]({{site.baseurl}}/docs/example-catalog/non-git-checkout/)

## Build/push examples

Codefresh has native support for [building]({{site.baseurl}}/docs/pipelines/steps/build/) and [pushing]({{site.baseurl}}/docs/pipelines/steps/push/) Docker containers.  
You can also compile traditional applications that are not Dockerized yet.

- [Build an Image with the Dockerfile in root directory]({{site.baseurl}}/docs/example-catalog/build-an-image-dockerfile-in-root-directory/)
- [Build an Image by specifying the Dockerfile location]({{site.baseurl}}/docs/example-catalog/build-an-image-specify-dockerfile-location)
- [Build an Image from a different Git repository]({{site.baseurl}}/docs/example-catalog/build-an-image-from-a-different-git-repository)
- [Build and Push an Image]({{site.baseurl}}/docs/example-catalog/build-and-push-an-image)
- [Build an Image with build arguments]({{site.baseurl}}/docs/example-catalog/build-an-image-with-build-arguments)
- [Share data between steps]({{site.baseurl}}/docs/example-catalog/shared-volumes-between-builds)
- [Upload or download from a Google Storage Bucket]({{site.baseurl}}/docs/example-catalog/uploading-or-downloading-from-gs/)
- [Get Short SHA ID and use it in a CI process]({{site.baseurl}}/docs/example-catalog/get-short-sha-id-and-use-it-in-a-ci-process)
- [Call a CD pipeline from a CI pipeline]({{site.baseurl}}/docs/example-catalog/call-child-pipelines)
- [Trigger a Kubernetes Deployment from a Dockerhub Push Event]({{site.baseurl}}/docs/example-catalog/trigger-a-k8s-deployment-from-docker-registry/)

## Unit and integration test examples

Codefresh has support for both [unit]({{site.baseurl}}/docs/testing/unit-tests/) and [integration tests]({{site.baseurl}}/docs/testing/integration-tests/) as well as [test reporting]({{site.baseurl}}/docs/testing/test-reports/).

- [Run unit tests]({{site.baseurl}}/docs/example-catalog/run-unit-tests) 
- [Run integration tests]({{site.baseurl}}/docs/example-catalog/run-integration-tests/) 
- [Run integration tests with MongoDB]({{site.baseurl}}/docs/example-catalog/integration-tests-with-mongo/) 
- [Run integration tests with MySQL]({{site.baseurl}}/docs/example-catalog/integration-tests-with-mysql/) 
- [Run integration tests with PostgreSQL]({{site.baseurl}}/docs/example-catalog/integration-tests-with-postgres/) 
- [Run integration tests with Redis]({{site.baseurl}}/docs/example-catalog/integration-tests-with-redis/) 
- [Populate a database with existing data]({{site.baseurl}}/docs/example-catalog/populate-a-database-with-existing-data) 

- [Shared volumes of service from composition step for other yml steps]({{site.baseurl}}/docs/example-catalog/shared-volumes-of-service-from-composition-step-for-other-yml-steps)
- [Launch Composition]({{site.baseurl}}/docs/example-catalog/launch-composition) 
- [Launch Composition and define Service Environment variables using a file]({{site.baseurl}}/docs/example-catalog/launching-a-composition-and-defining-a-service-environment-variables-using-a-file) 
- [Run multiple kinds of unit tests using fan-in-fan-out parallel pipeline]({{site.baseurl}}/docs/example-catalog/fan-in-fan-out) 

## Code coverage examples

- [Run coverage reports with Codecov]({{site.baseurl}}/docs/example-catalog/codecov-testing) 
- [Run coverage reports with Coveralls]({{site.baseurl}}/docs/example-catalog/coveralls-testing) 
- [Run coverage reports with Codacy]({{site.baseurl}}/docs/example-catalog/codacy-testing) 

## Secrets examples

Codefresh can automatically export secret key-value pairs using the Vault plugin from the [Step Marketplace](https://codefresh.io/steps/step/vault).

- [Vault secrets in the Pipeline]({{site.baseurl}}/docs/example-catalog/vault-secrets-in-the-pipeline)
- [Decryption with Mozilla SOPS]({{site.baseurl}}/docs/example-catalog/decryption-with-mozilla-sops)
- [GitOps with Bitnami sealed secrets]({{site.baseurl}}/docs/example-catalog/gitops-secrets)

## Preview environment examples

Codefresh can automatically launch environments (powered by Docker swarm) to [preview a Pull Reqest or feature]({{site.baseurl}}/docs/getting-started/on-demand-environments/). The definition of the environment can come from an [existing composition]({{site.baseurl}}/docs/testing/create-composition/), a docker-compose file or an inline YAML. Preview environments can be launched manually or [automatically from pipelines]({{site.baseurl}}/docs/pipelines/steps/launch-composition/).

- [MongoDB preload data]({{site.baseurl}}/docs/example-catalog/import-data-to-mongodb/) 
- [NodeJS + Angular2 + MongoDB]({{site.baseurl}}/docs/example-catalog/nodejs-angular2-mongodb/) 
- [NGINX Basic Auth]({{site.baseurl}}/docs/example-catalog/secure-a-docker-container-using-http-basic-auth/)
- [Spring Boot + Kafka + Zookeeper]({{site.baseurl}}/docs/example-catalog/spring-boot-kafka-zookeeper/)
- [Web terminal]({{site.baseurl}}/docs/example-catalog/web-terminal/)

## Deployment examples

Codefresh can deploy to any platform such as VMs, FTP/SSH/S3 sites, app servers, but of course it has great support for [Kubernetes clusters]({{site.baseurl}}/docs/deploy-to-kubernetes/deployment-options-to-kubernetes/) and [Helm releases]({{site.baseurl}}/docs/new-helm/helm-releases-management/):

- [Deploy to a VM with packer]({{site.baseurl}}/docs/example-catalog/packer-gcloud/) 
- [Deploy to a VM with FTP]({{site.baseurl}}/docs/example-catalog/transferring-php-ftp)
- [Deploy to Tomcat using SCP]({{site.baseurl}}/docs/example-catalog/deploy-to-tomcat-via-scp)
- [Deploy Demochat to a Kubernetes cluster]({{site.baseurl}}/docs/deploy-to-kubernetes/codefresh-kubernetes-integration-demochat-example/) 
- [Use kubectl as part of freestyle step]({{site.baseurl}}/docs/example-catalog/use-kubectl-as-part-of-freestyle-step) 
- [Deploy with Kustomize]({{site.baseurl}}/docs/example-catalog/deploy-with-kustomize)
- [Deploy with Helm]({{site.baseurl}}/docs/example-catalog/helm) 
- [Deploy with Terraform]({{site.baseurl}}/docs/example-catalog/terraform) 
- [Deploy with Pulumi]({{site.baseurl}}/docs/example-catalog/pulumi) 
- [Deploy to Nomad]({{site.baseurl}}/docs/example-catalog/nomad)
- [Deploy to Heroku]({{site.baseurl}}/docs/example-catalog/deploy-to-heroku/)
- [Deploy to Docker swarm]({{site.baseurl}}/docs/example-catalog/docker-swarm/)
- [Deploy to Elastic Beanstalk]({{site.baseurl}}/docs/example-catalog/elastic-beanstalk/)
- [Deploy to Amazon ECS/Fargate]({{site.baseurl}}/docs/example-catalog/amazon-ecs/)

## Notification examples

- [Send notification to Slack]({{site.baseurl}}/docs/example-catalog/sending-the-notification-to-slack)
- [Send notification to Jira]({{site.baseurl}}/docs/example-catalog/sending-the-notification-to-jira)
