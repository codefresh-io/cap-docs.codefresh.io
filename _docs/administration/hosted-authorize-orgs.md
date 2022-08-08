---
title: "Authorize organizations"
description: ""
group: runtime
toc: true
---

If your Git provider has an OAuth application for Codefresh, you need to authorize access to the app's organizations/projects to see them in Codefresh.

### Authorize organizations in GitHub

Request or grant access to the organizations defined for the OAuth Codefresh application.

1. Go to your profile in GitHub, and then select **Settings**.
1. From the sidebar, select **Applications**, and then click the **Authorized OAuth Apps** tab.
1. Click the **Codefresh** application. 
1. In the list of organizations that appears, click **Request** or **Grant**, for every organization to authorize as relevant. 

{% include
image.html
lightbox="true"
file="/images/administration/authorize-github-oauth-apps.png"
url="/images/administration/authorize-github-oauth-apps.png"
alt="Authoize "
caption="Runtime components for hosted runtime"
max-width="70%"
%}
