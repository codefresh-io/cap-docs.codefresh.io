---
title: "Using Codefresh for CI"
description: "Continuous integration (CI) with Codefresh pipelines"
group: getting-started
toc: true
---

Focus on: 
Building Docker images
Compiling code
Running unit tests
Running integration tests
Security scans
Code quality

CI platform/ tools


## Docker images
Building a Docker image from the source code is probably the most common and  basic requirement for a CI pipeline.
The `build` step in Codefresh allows you to build a Docker image in a completely declariative manner, and to automatically  it to your default Docker registry without any configuration.
Building a Dockerfile in a pipeline works in the same way as building the Dockerfile locally on your workstation.
The Images dashoabord displays all the images. The images are enrichmed with informatoin suach as the Git brnach that created the image and the Git has witht eh last commit.
For more on the build step see XREF
Here are other links that
Examples
Docker registries

## Code compilation

## Unit testing
Codefresh supports all testing frameworks, including mocking frameworks, for all popular programming languages. Easily run unit tests on the source code of the application for every commit or pull request (PR) through our freestyle step in pipelines. 

Run any type of unit tests in Codefresh pipelines, from smoke tests in a dockerfile, to tests with external or application images for simple applications, and evenrun them on a special testing image for complex applications.
You can create test reports and view them whenever you need. 

More links TBD
[Example catalog]({{site.baseurl}}/docs/example-catalog/ci-examples/run-unit-tests/)


## Integration testing
Compared to unit tests that run on the source code, integration tests run on the application itself. You need to either launch the application itself, or one or more external services such as a database.  
In Codefresh, you can launch these sidecar containers within the pipeline through compositions and service containers.


More links TBD
[Example catalog]({{site.baseurl}}/docs/example-catalog/ci-examples/run-integrations-tests/).

## Security scanning
Integrate Codefresh with any security scanning platform that scans source code or Docker images for vulnerabilities.
Codefresh can integrate with Any security solution 

A freestyle step as long as the scanning solution offers any of :Because you can insert a scanning step anywhere in your pipeline, you have great flexibility on when a security scan is happening. Common strategies are:

Scanning the source code before being packaged in a Container
Scanning a container before it is being stored to a registry
Scanning a container before being deployed to production
A Combination of the above
By attaching  Analayis reports to Codefresh builds using our test reportingCodefresh offers the capability to store your test results for every build and view them at any point in time.
Our plugin direction Codefresh has ready-to-use Docker images for serverl securoiry platforms such as Anchore, Aqua Security

More links TBD

## Code quality coverage
Good quality code is central to any CI platform or tool, and Codefresh integrates with the top code quality platforms/tools in the market.  track code coverage (Coverall), inspect code quaility (SonarQube) and generate code coverage analysis reports (Codecov). 
With three steps: 
set up the integrations
copy and paste the ready-to-use step for your platform/tool into your pipeline from Our plugins page  
Reference them by name in the pipeline step, and view the updated reports in the respective UIs.

You can also attach the analysis reports to your pipeline builds using our test reports and with a simple click view the entire report inthe Codefresh UI.   

More links TBD
