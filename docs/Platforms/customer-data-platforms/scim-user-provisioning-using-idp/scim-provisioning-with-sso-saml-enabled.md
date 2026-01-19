---
title: SCIM Provisioning With SSO (SAML Enabled)
excerpt: >-
  Learn to set up SCIM provisioning with SAML SSO to automate CleverTap user
  access.
deprecated: false
hidden: false
metadata:
  robots: index
---
# Overview

Use this setup if your organization already uses **SAML-based SSO** and wants **SCIM** to manage user lifecycle actions such as provisioning and deprovisioning.  
In this setup, **SCIM controls user creation and access updates**, while **SAML continues to handle authentication**.

This guide covers SCIM provisioning using an Identity Provider (IdP) **with SSO**. For SCIM provisioning using an IdP **without SSO**, refer to [SCIM Provisioning Without SSO](doc:scim-provisioning-without-sso).

# Prerequisites

Before configuring SCIM provisioning, ensure the following:

* SSO is configured and working in CleverTap.
* You have generated a **SCIM token**. Refer to [Generate SCIM Token for IdP-Based Provisioning](doc:scim-user-provisioning-using-idp#generate-scim-token-for-idp-based-provisioning).
* A CleverTap **Admin** has generated a SCIM token for your account.

Ensuring all this helps your IdP authenticate and sync users correctly with CleverTap.

# Integrate SCIM Provisioning With SSO

Integrate SCIM provisioning with your existing SAML SSO setup to automate user management while maintaining secure authentication. To do so, perform the following steps:

1. [Configure Single Sign On (SSO)](doc:scim-provisioning-with-sso-saml-enabled#configure-single-sign-on-sso)
2. [Configure the SCIM Application in Okta](doc:scim-provisioning-with-sso-saml-enabled#configure-scim-application-in-okta)
3. [Configure the SCIM Token](doc:scim-provisioning-with-sso-saml-enabled#configure-scim-token)
4. [Configure Required Attributes](doc:scim-provisioning-with-sso-saml-enabled#configure-required-attributes)
5. [Migrate from SAML to SCIM Provisioning](doc:scim-provisioning-with-sso-saml-enabled#migrate-from-saml-to-scim-provisioning)
6. [Managed Users](doc:scim-provisioning-with-sso-saml-enabled#managed-users)
7. [Verify User Sync and Login](doc:scim-provisioning-with-sso-saml-enabled#verify-user-sync-and-login)

This document uses Okta to illustrate the SCIM setup process. You can follow the same steps for other SCIM-compatible IdPs such as Azure AD or OneLogin.

## Configure Single Sign On (SSO)

Before enabling SCIM provisioning with SSO, ensure that SAML-based Single Sign-On (SSO) is already configured and working in CleverTap.

To configure SSO and map the required attributes, refer to the following guide:

* [Single Sign On (SSO)](https://docs.clevertap.com/docs/single-sign-on-sso)

Once SSO is successfully configured in your Identity Provider (for example, Okta), the SCIM configuration screen appears as shown in the image below.

<Image align="center" alt="SCIM Configuration Screen" border={true} caption="SCIM Configuration Screen" src="https://files.readme.io/52c72c2ef3f404a3dd8158ca0cc81ac85870c03dbc358f5669636f6e17ae7845-image.png" width="75% " />

## Configure SCIM Application in Okta

After setting up SSO, configure the IdP to use SCIM with SAML by performing the following steps:

1. In Okta, go to _Applications_ > _SCIM with SAML_ (the application created in the previous step) > _General_ > _App Settings_.
2. Click on **Edit**.
3. Change _Provisioning_ from _None_ to _SCIM_
4. Keep the default setting as is and click **Save**.

<Image align="center" alt="Configure SCIM Application in Okta" border={true} caption="Configure SCIM Application in Okta" src="https://files.readme.io/fa2b61f2f585b7c09a691e590ada90f6111f0bc84732a65c45203e5ebed59728-2025-12-29_15-55-45_1.gif" width="75% " />

## Configure SCIM Token

Enable SAML and link your Okta SCIM application to CleverTap using your SCIM token by performing the following steps:

1. Navigate to the _Provisioning_ tab, then open the _Integration_ section and click **Edit**.
2. Configure the following settings:
   * **SCIM Connector Base URL**: `https://<your_clevertap_domain>/nx/v2/scim/v2/idp`  
     (Example: `https://us1.dashboard.clevertap.com/nx/v2/scim/v2/idp`)

     <Callout icon="🚧" theme="warn">
       #### Region-Specific

       The CleverTap domain is environment- and region-specific. Do not copy the example URL directly. Always replace `<your_clevertap_domain>` with the domain for your region. Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.
     </Callout>
   * **User Identifier**: `userName`
   * **Supported Provisioning Actions**: Enable the following options by selecting the respective checkboxes:
     * **Import New Users and Profile Updates**
     * **Push New Users**
     * **Push Profile Updates**
     * **Push Groups**
   * **Authentication Mode**: `HTTP Header`
   * **HTTP Header Authorization**: `<SCIM_TOKEN>` (paste your CleverTap SCIM token). For more information, refer to [Generate SCIM Token for IdP-Based Provisioning](doc:scim-user-provisioning-using-idp#generate-scim-token-for-idp-based-provisioning).

<Image align="center" alt="SCIM Connector Configuration — Okta" border={true} caption="SCIM Connector Configuration - Okta" src="https://files.readme.io/d1c35a73ad811f4b4c08ea751caa81459b07e4e855605ab56c4b32484606cc1f-2025-12-29_15-59-26_1.gif" width="75% " />

3. Click **Test Connector Configuration**, and once successful, click **Save**.

<Image align="center" alt="Test Connector Configuration" border={true} caption="Test Connector Configuration" src="https://files.readme.io/910fae3bfb0c9fdecdf3b1305cb2df45ef8a3284b397771aa2f3e6906851f018-image.png" width="65% " />

## Configure Required Attributes

Add a custom attribute to define which CleverTap accounts each user can access.

* To add the attribute, refer to [Add `accounts` Attribute](doc:scim-provisioning-without-sso#add-the-accounts-attribute).
* To unmap and delete unnecessary attributes, refer to [Remove Unnecessary Attributes](doc:scim-provisioning-without-sso#remove-unnecessary-attributes).

The schema requires only the following attributes: `userName`, `givenName`, `familyName`, and `accounts`

<Image align="center" alt="SCIM Required Attributes" border={true} caption="SCIM Required Attributes" src="https://files.readme.io/25bbe8256518ef2d8ebe031832c2c35dc4c7c1297b343d45e4cde9ae78d74db0-image.png" width="75% " />

## Migrate from SAML to SCIM Provisioning

Ensure a clean transition to avoid conflicts between SCIM and SAML.

<Callout icon="⚠️" theme="warn">
  #### Important

  * Remove or unmap the SAML `accountList` attribute before enabling SCIM.
  * Avoid creating users with both SAML and SCIM simultaneously.
  * Migrate existing SSO users to SCIM provisioning for consistency.
</Callout>

### Delete `accountList` Attribute

If you plan to use **SCIM** for user provisioning, you must set the `accountList` attribute to **Not Mapped** in the **Edit User Assignment** screen. This prevents conflicts between **SAML-based SSO** and **SCIM provisioning**.

To Delete the `accountList` attribute:

1. In your IdP, go to _Profile Editor_
2. Click the **X** (Remove) icon next to the attribute name `accountList` to delete the Attribute.

<Image align="center" alt="Delete `accountList` Attribute" border={true} caption="Delete `accountList` Attribute" src="https://files.readme.io/3c30a0e685c9de0c07eac0edb4a5daf22bf9d52194bea8d8fa51ad966c74ab1c-2025-12-29_16-36-04_1.gif" width="75% " />

This ensures that user provisioning (creation, updates, and removal) is handled exclusively through SCIM.

### &#x20;Provision Existing SSO Users into SCIM

If SSO is already configured and users exist before enabling SCIM:

* Existing users are not automatically SCIM-managed.
* The IdP provides a Provision All option.

Using Provision All:

* Sends SCIM create calls for existing users.
* Does not immediately change access.
* Brings users under SCIM lifecycle management.

After this step, updates and access changes are managed through SCIM.

#### Provision Existing SSO Users

To provision existing SSO users into SCIM, follow these steps:

1. Complete the SCIM setup by following:
   1. [Configure SCIM Application in Okta](doc:scim-provisioning-with-sso-saml-enabled#configure-scim-application-in-okta)
   2. [Configure SCIM Token](doc:scim-provisioning-with-sso-saml-enabled#configure-scim-token)
2. After configuring the SCIM token, go to the _Assignments_ tab in your IdP.
3. Click **Provision Users** (this is a one-time action).
4. Confirm the action to successfully provision all existing users into SCIM.

<Image align="center" alt="Provision Existing SSO Users" border={true} caption="Provision Existing SSO Users" src="https://files.readme.io/b88c16261d944229934a57e29028c9f6a21d89c1279f86bbf003413c3d294547-2025-12-29_16-42-56_1.gif" width="75% " />

Once provisioning is complete, all existing SSO users are fully managed through SCIM.

## Managed Users

Managed Users define how CleverTap adds, updates, and deactivates users based on assignments made in your Identity Provider (IdP).

The IdP controls user lifecycle management. CleverTap does not require manual user management once SCIM provisioning is configured.

### Add or Update a User in CleverTap

You can add a new user or update an existing user by editing the user assignment in your IdP.

1. Go to _Applications_ > _Assignments_.
2. Click the **Edit** icon next to the assigned user.
3. Update the _JSON_ payload as required. For example, you can:
   * Add or modify roles.
   * Change the user status.

### Example JSON payload.

```json
{
  "accountId": "ACC123", // CleverTap ID
  "roles": "Editor",
  "teams": "Dev",
  "status": "Active"
}
```

4. Click **Save**.

After saving:

* The IdP fetches the user data from CleverTap.
* If the user already exists, CleverTap updates the user.
* If the user does not exist, CleverTap creates a new user.

No manual action is required in the CleverTap dashboard.

<Image align="center" alt="Add or Update a User" border={true} caption="Add or Update a User" src="https://files.readme.io/5bea8e55120e8cbeca0ea742ade865c6e71ca030e3788fdc92f35150c3fd9c64-2025-12-29_16-05-49_1.gif" width="75% " />

To **Delete User**, refer to [Delete User](doc:scim-provisioning-without-sso#delete-user).

## Verify User Sync and Login

After assigning users, verify that provisioning is working correctly and that CleverTap reflects your IdP updates in real time by performing the following steps:

1. In the CleverTap dashboard, go to _Settings_ > _Users_.
2. Search for the user you just assigned to check their provisioning status. The user’s status should display as _User Invited_ once provisioning is successful.
3. Confirm that updates in Okta (or your IdP) automatically sync and appear in CleverTap.

<Image align="center" alt="Verify User Sync and Login" border={true} caption="Verify User Sync and Login" src="https://files.readme.io/5a446d4834876b8a9b7757a12e8ea4fab519383241f089afb46b8cccfe91ab51-image.png" width="85% " />

If changes made in your IdP (for example, user deactivation or role updates) do not appear immediately in CleverTap, wait a few minutes for SCIM synchronization or trigger a manual sync in your IdP.

# Troubleshooting

| Issue           | Cause                         | Resolution                         |
| --------------- | ----------------------------- | ---------------------------------- |
| Duplicate users | Both SCIM and SAML are active | Disable SAML `accountList` mapping |
| Login fails     | Old SSO session               | Reset password or clear cookies    |
| Missing users   | Attribute mismatch            | Verify `accounts` mapping          |

# FAQ

The following are common questions and answers about SCIM:

### Why do first-time SCIM IdP users need to reset their password?

When a user is provisioned through SCIM for the first time (and SSO is not enabled), the account is created in CleverTap without a predefined password. To access the platform:

1. Go to the CleverTap login page and click Forgot Password.
2. Enter the registered email address.
3. Click the password reset link sent to their email to set a new password.

This is a one-time action required only for the user’s initial login. Once the password is set, subsequent logins use the new credentials unless the organization later enables SSO.
