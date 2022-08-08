---
title: "Authorize organizations/projects"
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
1. In the list of organizations that appears, for every organization you need to authorize, click **Request** or **Grant**, as relevant. 

{% include
image.html
lightbox="true"
file="/images/administration/authorize-github-oauth-apps.png"
url="/images/administration/authorize-github-oauth-apps.png"
alt="Authorize Codefresh organizations in GitHub"
caption="Authorize Codefresh organizations in GitHub"
max-width="70%"
%}
