



CSDP enrichment step for 3rd party ci tools
Background
Image enrichment is a crucial part of the CI/CD process to enhance the quality of deplyments. Without image enrichment, application images are limited to  physical are mostly one-dimennsional s they give build and version information. With image enrichment, application image  multip dimentions as they re layedred with pileine and i=workflows information. For example, Jita  images are you crate when you deply an application The information you add to the container images from any CI tool such as Jira, Git etc provide a weath of information 
Multi-dimentionsl as pects 

Codefresh supports image enrichment for both hybrid and hosted modes:

1. Hybrid CI Ops and CD Ops: In hybrid environments where you have the complete CI/CD If you use Codefresh, setting up CI integrations, connects the CI platform tools to Codefresh
Existing pipelines and workflows are conpatiable and not affected by the new integration. 


1. Hosed CD Ops
Here you have support for CD range. Regardless, you can set up
  Set up an integration between your CI tool and Codefresh. Codefresh uses the integration nam,e to retirive the data from each of the CI platofrms/tools and view 
  Today, we are enriching our images with information from jira, git etc via CSDP-metadata workflow template. This template contains all the logic of the enrichment in the image itself. 
  In this way to you can connect any CI tool to the Hosted GitOps. See 

### CI tools supported

Codefresh supports the following CI platforms/tools:


JIRA
Docker registries: DockerHub and Quay
GitHub Actions

We are working on supporting integrations for additional CI platforms/tools. Stay tuned for the release announcements. 

### How does CI Connect work?
Integrate your JIRA account with Codefresh for obseravbility and enrichment. Once set up, Codefresh pulls in the JITA information or issues associated with your deployments automatically. The details are shown in the Applications dashbaord 


Goals
Reduce the friction as much as possible of connecting user’s CI to Codefresh Managed Argo.

Provide full functionality of application dashboard

Show commit information as well as committer

link to build and deployment pipelines

show PRs that are included in this deployment

show Jira issues and their status and details for each deployment

Send deployment information back to issues include in this deployment




Additional Guidelines
Enrichment step today is split into three, image reporting, git info enrichment and Jira info enrichment. We want the user to send a single request to the Codefresh, and Codefresh will perform all the necessary actions in order to display the full information in our dashboard. This includes any future enhancements we might add in the future.

It is understood the new functionality might require the user to update their CI, with either new values to send or a new version of the step.  New capabilities should not break existing CI flows.

In order to have a seamless flow the user, we want to use integrations already defined in the system,  and not have them input credentials asl part of the CI step. 

As part of the enrichment step we need to integrate with 3rd party systems

 Git → git data

Jira → Jira issue data, Jira deployment notification

Docker registries → reporting image to platform



Users come to Codefresh because they want to enjoy the full benefits of GitOps. While it’s been possible to use Codefresh GitOps without CI in place, automated builds, tests, approvals, and other CI benefits, have a huge impact on the quality of applications deployed. Exposing the metadata, trends, and logs from CI systems as part of the deployment process makes it easier to perform audits, track down the root cause of failures, and maintain a high level of security.

By opening up Codefresh GitOps to integrate with 3rd party CI systems we can skip the work and overhead of converting CI pipelines so users can start enjoying the full benefits of GitOps and Codefresh dashboards without CI migration. Jenkins and Github Actions are just a starting point, we expect to release support for additional CI providers shortly and the integration is architected in such a way that it is relatively easy to add and maintain additional support. Bring your CI pipelines and focus on where you’ll get the most value: adopting GitOps to deploy applications.

 

Basically, both of them are valid, but i tend to think we need to use the second option just for easier upgrades, fixes changes etc. 

If the core logic will not sit on our hand, it will be very hard to maintain it, and also probably require more time to add a new CI tool which should be pretty seamless with the second approach. 

Limitations
As we mentioned earlier, one of the requirements is remove the need to provide 3th parties application credentials and use the integrations mechanism. 

But as you know, the credentials going to be stored in the user cluster which might be not accessible to the outside world (in some cases)

We will start from taking an assumption that the customer cluster is accessible to the outside world (in cases its not correct we will probably need to some similar solution as we did with webhook relay)

Execution 
 

We can take two approaches here

We can continue to use the existing csdp metadata step, and basically just run this step in the customer cluster (we will have to refactor it in any case)

Extract the logic and have it as part of our code