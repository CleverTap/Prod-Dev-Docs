---
title: SCIM Provisioning Without SSO
excerpt: Learn how to set up SCIM user provisioning in CleverTap without SSO.
deprecated: false
hidden: false
metadata:
  robots: index
---
# Overview

CleverTap supports System for Cross-domain Identity Management (SCIM) to automate user provisioning when Single Sign-On (SSO) is not in use. Your Identity Provider (IdP), such as Okta or Azure AD, communicates directly with CleverTap to synchronize user creation, updates, and deactivation.

This guide covers SCIM provisioning using an Identity Provider (IdP) **without SSO**.  For SCIM provisioning using an IdP **with SSO (SAML enabled)**, refer to [SCIM Provisioning With SSO (SAML Enabled)](doc:scim-provisioning-with-sso-saml-enabled).

# Prerequisites

Before configuring SCIM provisioning, ensure the following:

* A CleverTap **Admin** has generated a SCIM token for your account.
* You have generated a **SCIM token**. For more information, refer to [Generate SCIM Token for IdP-Based Provisioning](doc:scim-user-provisioning-using-idp#generate-scim-token-for-idp-based-provisioning).
* Your IdP supports **SCIM 2.0**.

Ensuring all this helps your IdP authenticate and sync users correctly with CleverTap.

# Integrate SCIM Provisioning Without SSO

To automate user provisioning without Single Sign-On (SSO), perform the following steps:

1. [Configure SCIM Application in Okta](doc:scim-provisioning-without-sso#configure-scim-application-in-okta)
2. [Configure SCIM Token](doc:scim-provisioning-without-sso#configure-scim-token)
3. [Configure Required Attributes](doc:scim-provisioning-without-sso#configure-required-attributes)
4. [Add and Assign Users](https://developer.clevertap.com/docs/scim-provisioning-without-sso#add-and-assign-users)
5. [Managed Users](doc:scim-provisioning-without-sso#managed-users)
6. [Verify Provisioning Status](doc:scim-provisioning-without-sso#step-5-verify-provisioning-status)

This document uses Okta to illustrate the SCIM setup process. You can follow the same steps for other SCIM-compatible IdPs such as Azure AD or OneLogin.

## Configure SCIM Application in Okta

Set up an application in Okta that connects to CleverTap’s SCIM endpoint by performing the following steps:

1. In Okta, go to _Applications_ > _Browse App Integration Catalog_.
2. Search for **SCIM 2.0 Test App (Header Auth)** and click **+ Add Integration**.
3. Enter your _Application label_ and click **Next**.
4. Keep the default setting as is and click **Done**.

<Image align="center" alt="Okta SCIM Integration" border={true} caption="Okta SCIM Integration" src="https://files.readme.io/44a89abb0d90b22519aad61e2ca2ee0455f0dd58eaa5e6c4393b5c306d973c05-2025-12-29_14-29-37_1.gif" />

## Configure SCIM Token

Link your Okta SCIM application to CleverTap using your SCIM token by performing the following steps:

1. In Okta, open the SCIM app created in the [Configure SCIM Application in Okta](doc:scim-provisioning-without-sso#configure-scim-application-in-okta) step, and go to _Provisioning_ > **Configure API Integration**.
2. Enable **API Integration**, and enter the following:

   * **Base URL**: `https://<your_clevertap_domain>/nx/v2/scim/v2/idp`  
     (Example: `https://us1.dashboard.clevertap.com/nx/v2/scim/v2/idp`)

   <Callout icon="🚧" theme="warn">
     #### Region-Specific

     The CleverTap domain is environment- and region-specific. Do not copy the example URL directly. Always replace `<your_clevertap_domain>` with the domain for your region. Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.
   </Callout>

   * **API Token**: `Bearer <SCIM_TOKEN>` (include the prefix and space)

   For more information, refer to [Generate SCIM Token for IdP-Based Provisioning](doc:scim-user-provisioning-using-idp#generate-scim-token-for-idp-based-provisioning).

<Image align="center" alt="Configure SCIM Token" border={true} caption="Configure SCIM Token" src="https://files.readme.io/a30b4f6678bda211e350c38e1ae3b5ce687b62c9703920aec1832e8063a1d4da-2025-12-29_14-36-41_1.gif" />

3. Click **Test API Credentials**.
   * **Green indicator**: Successful connection

     <Image align="center" alt="Successful connection" border={true} caption="Successful connection" src="https://files.readme.io/4a867fac913ea6466eebbfb95366f4c1ceb2ec2e13b7f056cf01c5dad3de24c5-image.png" width="60% " />
   * **Red indicator**: Configuration error

     <Image align="center" alt="Configuration error" border={true} caption="Configuration error" src="https://files.readme.io/69b559e695744b57eed29674b93a8e13a387e26220b8eeaf5ac175603cec0231-image.png" width="55% " />
4. Click **Save**
5. Go to _To App_ and click on **Edit**
6. Enable the following provisioning actions supported by CleverTap by selecting the corresponding checkboxes and click **Save**:
   * Create Users
   * Update Users Attributes
   * Deactivate Users

<Image align="center" alt="Provisioning Actions" border={true} caption="Provisioning Actions" src="https://files.readme.io/af4bde850483356709fd728db414d0255201e09602055ac7f9e3c29ddbccad45-2025-12-29_14-42-36_1.gif" width="75% " />

<Callout icon="📘" theme="info">
  #### Security and Login Notes

  CleverTap never stores or synchronizes IdP passwords. Authentication is managed exclusively through your Identity Provider.
</Callout>

## Configure Required Attributes

In this step, you’ll define which user attributes your Identity Provider (IdP) sends to CleverTap.
By default, your IdP may include many SCIM attributes, but CleverTap only requires a few for successful provisioning.

To configure attributes, follow these two key sub-steps:

* [Add the `accounts` Attribute](doc:scim-provisioning-without-sso#add-accounts-attribute): Create a custom field that defines user access to CleverTap accounts.
* [Remove Unnecessary Attributes](doc:scim-user-provisioning-using-api?isFramePreview=true#remove-unnecessary-attributes): Unmap and delete attributes that CleverTap doesn’t use.

### Required Attributes

Keep the following attributes mapped and active; these fields are mandatory for user provisioning in CleverTap.

| Attribute    | Type         | Description                                 |
| ------------ | ------------ | ------------------------------------------- |
| `userName`   | String       | Unique identifier for the user              |
| `givenName`  | String       | User’s first name                           |
| `familyName` | String       | User’s last name                            |
| `accounts`   | String array | Lists CleverTap account IDs (**Mandatory**) |

All other attributes can be safely unmapped and removed to simplify your SCIM configuration. To unmap and remove, refer to [Remove Unnecessary Attributes](https://staging.docs.dev.clevertap.net/docs/scim-provisioning-without-sso#remove-unnecessary-attributes).

### Add `accounts` Attribute

The `accounts` attribute indicates CleverTap, which accounts for each user should have access to.
You need to create it manually in your IdP as a custom attribute.

1. Go to _To App_
2. Click on **Go to Profile Editor** > **+ Add Attribute**.
3. Fill in the following details and click **Save**:

   * **Data type**: string array
   * **Display name**: Accounts
   * **Variable name**: `accounts`
   * **External name**: `urn:ietf:params:scim:schemas:core:2.0:User`
   * **Description**: (Optional) add as needed or leave blank
   * **Enum**: Leave empty
   * **Attribute required**: Yes (In case of SCIM Provisioning With SSO (SAML Enabled) **Not required**)
   * **Attribute type**: Personal

<Image align="center" alt="Add Attribute" border={true} caption="Add Attribute" src="https://files.readme.io/2311bb1b74df611cfeb2ab62df8376b4fb57860f9b5ff4c99bed7b066211a0e4-2025-12-29_15-37-10_1.gif" width="75% " />

<Callout icon="📘" theme="info">
  #### Recommendation

  Define this field as a **String array** so a single user can be linked to multiple CleverTap accounts, for example, a parent account and its associated child accounts.
  When you add a parent account ID to a user, CleverTap automatically recognizes and links access to its related child accounts.
  This ensures that user roles and permissions cascade properly across all connected accounts without requiring separate mappings.
</Callout>

### Remove Unnecessary Attributes

Your IdP may include default attributes that CleverTap does not use. To remove them, follow these steps:

1. In your IdP, go to _Profile Editor_ > _Mappings_.
2. From the dropdown menu, select _Do not map_ for the specific attribute.
3. Click **Save Mappings** to apply the changes.
4. After unmapping, click the **X** (Remove) icon next to the attribute name to _Delete Attribute_.
5. Click **Save Attribute** to confirm deletion.

<Image align="center" alt="Remove Attributes" border={true} caption="Remove Attributes" src="https://files.readme.io/cfd9933a83a8f0996c705e9e71926bb412667b92e77f9bb475e657cc6046c5f5-2025-12-29_15-14-09_1.gif" width="75% " />

<Callout icon="🚧" theme="warn">
  #### Important

  * You cannot delete an attribute while it is still mapped. Always unmap it first, then remove it.
</Callout>

## Add and Assign Users

Add users in your organization’s IdP and assign them access to CleverTap. This ensures each user is automatically created, updated, and managed in CleverTap based on your IdP configuration.

### Add User in IdP

Add new users in your IdP so they can be provisioned automatically in CleverTap through SCIM. To do so, perform the following steps:

1. Go to _Directory_ > _People_  > _Add Person_.
2. Enter the user’s details such as **First Name**, **Last Name**, **Username**, and **Primary Email**.
3. Under **Activation**, select **Activate now** if you want the user to be active immediately.
4. Select **I will set password** and optionally check **User must change password on first login** for added security.
5. Click **Save** to create the user.

<Image align="center" alt="Add User in IdP" border={true} caption="Add User in IdP" src="https://files.readme.io/3cab90dddf4ee0822fbe17e6a4f3dc1b6846b7986bc4803284f1b8d766546918-image.png" width="65% " />

### Assign User

Assigning a user links their IdP profile to CleverTap, allowing automatic provisioning of roles, permissions, and account access based on the JSON mapping. Assign users from your IdP to the CleverTap SCIM application by performing the following steps:

1. Go to _Applications_ > _Assignments_ > then open the **Assign** dropdown and select _Assign to People_.
2. A list of users in your application will appear. Click **Assign** next to the specific user you want to provision.
3. In the _Account_, click **Add Another** and paste the JSON payload. Refer to the [Example JSON payload](https://staging.docs.dev.clevertap.net/docs/scim-provisioning-without-sso#example-json-payload).

If the user already exists, CleverTap automatically updates their details. If not, a new user record will be created in CleverTap.

#### Example JSON Payload:

```json
{
  "accountId": "ACC123", // CleverTap ID
  "roles": "Editor",
  "teams": "Marketing",
  "status": "Active"
}
```

<Image align="center" alt="Assign User" border={true} caption="Assign User" src="https://files.readme.io/6a005a9b1ead6be37bdb9d3268db9db70d1d304a8f258beeccb7676f7866cfcc-2025-12-29_15-22-46_1.gif" width="75% " />

<Callout icon="⚠️" theme="warn">
  #### Important

  * Ensure valid JSON syntax (no trailing commas or spaces).
  * Invalid payloads stop provisioning.
  * Due to IdP limitations, you may need to trigger the provisioning workflow manually (for example, by editing and saving the JSON or removing a space in the UI).
</Callout>

Once users are assigned and provisioned, CleverTap continues to manage user access based on IdP changes. The following section explains how add, update, and revoke actions work after provisioning.

## Managed Users

Managed Users define how CleverTap adds, updates, and deactivates users based on assignments made in your Identity Provider (IdP).

User lifecycle management is fully controlled by the IdP. CleverTap does not require manual user management once SCIM provisioning is configured.

### Add or Update a User in CleverTap

You can add a new user or update an existing user by editing the user assignment in your IdP.

1. Go to _Applications_ > _Assignments_.
2. Click the Edit icon next to the assigned user.
3. Update the JSON payload as required. For example, you can:
   * Add or modify roles.
   * Change the user status.
4. Click **Save**.

After saving:

* The IdP fetches the user data from CleverTap.
* If the user already exists, CleverTap updates the user.
* If the user does not exist, CleverTap creates a new user.

No manual action is required in the CleverTap dashboard.

<Image align="center" alt="Add or Update a User in CleverTap" border={true} caption="Add or Update a User in CleverTap" src="https://files.readme.io/0f0a01cf8c4a6f79f67d0f5792aa842b40076039635321c8faf6b5ff038307b6-2025-12-29_15-27-22_1.gif" width="75% " />

<Callout icon="📘" theme="info">
  #### Verify User Provisioning Status

  After saving the assignment:

  * If you see a **red error indicator**, the provisioning request has **failed**.
  * If you do **not** see any red indicator, the user has been **successfully added or updated** in CleverTap.
</Callout>

### Delete User

To delete a user from the CleverTap dashboard, remove the user’s assignment from your IdP.

1. Go to _Applications_ > _Assignments_.
2. Click the **X** icon next to the assigned user to delete the user.

Once the user is removed:

* The user will be removed from the CleverTap dashboard.

<Image align="center" alt="Delete User Access" border={true} caption="Delete User Access" src="https://files.readme.io/94dd1b1fee231cb4ccdce25c61f4f855d6caebd732e088105d1ee6061548bf0d-2025-12-29_15-28-59_1.gif" width="75% " />

## Verify Provisioning Status

After assigning users, verify that provisioning is working correctly and that CleverTap reflects your IdP updates in real time.

1. In the CleverTap dashboard, go to _Settings_ > _Users_.
2. Search for the user you just assigned to check their provisioning status. The user’s status should display as **Active** once provisioning is successful.
3. Confirm that updates in Okta (or your IdP) automatically sync and appear in CleverTap.

<Image align="center" alt="Verify User Sync and Login" border={true} caption="Verify User Sync and Login" src="https://files.readme.io/5a446d4834876b8a9b7757a12e8ea4fab519383241f089afb46b8cccfe91ab51-image.png" />

If changes made in your IdP (for example, user deactivation or role updates) do not appear immediately in CleverTap, wait a few minutes for SCIM synchronization or trigger a manual sync in your IdP.

# Troubleshooting

| Issue              | Cause              | Resolution                     |
| ------------------ | ------------------ | ------------------------------ |
| Red test indicator | Wrong URL or token | Verify SCIM endpoint and token |
| User missing       | Invalid JSON       | Correct payload                |
| Token error        | Expired token      | Regenerate and update in IdP   |
