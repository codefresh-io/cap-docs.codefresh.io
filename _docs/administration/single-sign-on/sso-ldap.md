---
title: "LDAP Single Sign-On (SSO)"
description: " "
group: administration
sub_group: single-sign-on
redirect_from:
  - /docs/enterprise/single-sign-on/sso-ldap/
canonical_url: 'https://codefresh.io/docs/docs/single-sign-on/ldap/'
toc: true
---

To configure SSO for LDAP in Codefresh, you need to first create a user on your LDAP server who has permissions to search other users. Usually,
this user is an LDAP admin.  
Make sure also that you know the scope of the search, that is, where users to search for are located in the LDAP hierarchy.

1. In the Codefresh UI, go to [Single Sign-On](https://g.codefresh.io/2.0/account-settings/single-sign-on){:target="\_blank"}.
1. Select **+ Add Single Sign-On**, **LDAP**, and then **Next**.

   {% include image.html 
  lightbox="true" 
  file="/images/administration/sso/ldap/sso-csdp-ldap.png" 
  url="/images/administration/sso/ldap/sso-csdp-ldap.png"
  alt="SSO settings for LDAP in Codefresh"
  caption="SSO settings for LDAP in Codefresh"
  max-width="30%"
  %} 

{:start="3"}
1. Enter the following:
  * **Client Name**: For auto-generation, leave empty. Codefresh generates the client name once you save the settings.
  * **Display Name**: Any meaningful name for this integration.
  * **Password**: The password of the user defined in **Distinguished name** that will be used to search other users.
  * **LDAP Server URL**: Codefresh supports both `ldap` and `ldaps` protocols. For `ldaps`, you also need a certificate.
  * **Distinguished name**: The username to be used to search other users in LDAP notation (combination of `cn`, `ou`,`dc`).
  * **Search base**: The search-user scope in LDAP notation.
  * **Search filter**: The attribute by which the user will be searched on the LDAP server. By default, set to `uid`. For the Azure LDAP server, set this field to `sAMAccountName`.
  * **Certificate**: The security certificate of the LDAP server for `ldaps` only. Paste the value directly on the field. Do not convert to base64 or any other encoding by hand.  Leave the field empty if you use `ldap`.

{:start="4"} 
1. Select **+ Add**. LDAP users can log in to Codefresh.

>Each user who logs in to Codefresh must:
  1. Have a defined email address on the LDAP server
  1. Use the same email address as defined on the LDAP server
  1. Use as login information, the LDAP email, password, and `cn` value of username
