---
title: Unity Multi-Environment Credential Support
excerpt: >-
  Learn how to configure and manage multiple CleverTap environments in a Unity
  project using the multi-environment credential system.
deprecated: false
hidden: false
metadata:
  robots: index
---
# Overview

The CleverTap Unity SDK 5.5.0 and above supports _multi-environment credential configuration_, allowing you to manage multiple CleverTap environments, such as _TEST_, _DEV_, _STAGE_, _PROD_, and custom environments, within a single Unity project.

This capability helps teams maintain clean separation between development, testing, staging, and production environments while reducing the risk of misconfigured release builds.

Use this feature if you:

* Work with multiple CleverTap projects.
* Maintain separate credentials for non-production environments.
* Need a controlled environment switching during development.

## Key Capabilities

The CleverTap Unity SDK provides the following capabilities for managing multiple environments.

* Configure separate CleverTap credentials for multiple environments.
* Define custom environment keys beyond the default options.
* Switch environments during development builds.
* Automatically use the default environment in release builds.
* Supported on both _iOS_ and _Android_.
* Managed entirely through the Unity Editor.

# Prerequisites

Before configuring multi-environment credentials, ensure that:

* The CleverTap Unity SDK 5.5.0 and above is installed in your Unity project.
* You can access **CleverTap Account ID**, **Account Token**, and **Region** values from the CleverTap dashboard.

For initial SDK installation, refer to [Unity SDK Quick Start Guide](doc:unity-sdk-quick-start-guide-sdk-v300).

# Configuration

This section explains how to configure multiple CleverTap environments and manage environment credentials using the Unity Editor.

1. [Access the CleverTap Settings](doc:unity-multi-environment-credential-support#access-the-clevertap-settings)
2. [Configure Default Environments](doc:unity-multi-environment-credential-support#configure-default-environments)
3. [Create Custom Environments](doc:unity-multi-environment-credential-support#create-custom-environments)
4. [Save Environment Settings](doc:unity-multi-environment-credential-support#save-environment-settings)

## Access the CleverTap Settings

Open the CleverTap Settings window in the Unity Editor using one of the following methods:

* From the menu: _Assets_ > _CleverTap Settings_.

  Or
* Right-click in the Project window and select _CleverTap Settings_.

<Image align="center" border={true} caption="Multi-Environment Credential Support" src="https://files.readme.io/52b0090b299a60166716af9d8ac1153241cb7e622376cf0b16dfa3bda186cf3c-image.png" width="600px" />

## Configure Default Environments

Configure each predefined environment (_TEST_, _DEV_, _STAGE_, _PROD_):

1. Select the default Environment from the dropdown.
2. Expand each environment in the list to configure:
   * CleverTap Account Id: Your CleverTap Project ID
   * CleverTap Account Token: Your CleverTap Project Token
   * CleverTap Account Region (Optional): Region code for regional endpoints
   * CleverTap Proxy Domain (Optional): Custom proxy domain
   * CleverTap Spiky Proxy Domain (Optional): Spiky proxy domain

The default environment is used automatically in release builds.

## Create Custom Environments

You can define additional environment keys beyond the predefined set. This is useful when your workflow includes environments such as _QA_, _UAT_, or _DEMO_.

To add custom environments, extend the `CleverTapEnvironmentKey` enum.

```csharp
// Add to CleverTapEnvironment.cs
public enum CleverTapEnvironmentKey
{
    TEST = 0,
    DEV = 1,
    STAGE = 2,
    PROD = 3,
    QA = 4,          // Custom QA environment
    UAT = 5,         // User Acceptance Testing
    DEMO = 6         // Demo environment
}
```

After adding a new key, it becomes available for selection in the CleverTap Settings window.

## Save Environment Settings

To persist your configuration:

1. In _Assets_ >_CleverTapSettings_, enable `CleverTapSettingsSaveToJSON`.
2. Click **Save Settings**.

The SDK generates the following file:

* `StreamingAssets/CleverTapSettings.json`

This file is read at runtime to determine the active environment configuration.

## Runtime Environment Switching

This section explains how to switch between CleverTap environments programmatically at runtime.

### Development Builds Only

You can switch environments programmatically during development builds.

```csharp
CleverTap.SelectCleverTapEnvironmentCredential(CleverTapEnvironmentKey.DEV);
```

> 📘 Build Behavior
>
> Runtime environment switching is supported only in development builds. Release builds always use the configured default environment.

# Migrating from Legacy Single-Environment Settings

To migrate projects that were previously configured with a single CleverTap environment to the new multi-environment credential model.

> 🚧 Migration Required
>
> If you previously used the single-environment CleverTap configuration, you must re-enter your credentials using the new multi-environment settings. Credentials are stored differently to support multiple environments.

## Migration Steps

Follow these steps to move your existing CleverTap credentials into the new multi-environment configuration.

1. [Open the CleverTap Settings Window](doc:unity-multi-environment-credential-support#open-the-clevertap-settings-window)
2. [Locate Existing Credentials](doc:unity-multi-environment-credential-support#locate-existing-credentials)
3. [Configure the PROD Environment (Required)](doc:unity-multi-environment-credential-support#configure-the-prod-environment-required)

### Open the CleverTap Settings Window

Access the CleverTap Settings in the Unity Editor to begin the migration process.

* Go to _Assets_ > _CleverTap Settings_, or search for _CleverTap Settings_ in the Project window.

### Locate Existing Credentials

Identify your previously configured CleverTap credentials so you can reuse them in the new environment setup.

* Find your current CleverTap credentials from one of these locations:
* Source Location old Settings File `Assets/CleverTapSettings.asset` (if exists)

Use these values to populate the new environment configuration.

### Configure the PROD Environment (Required)

Re-enter your production credentials in the PROD environment to ensure correct behavior in release builds.

1. In the CleverTap Settings window, select the default environment to _PROD_.
2. Expand Environments _PROD_.
   * If PROD does not exist, add a new environment entry and set Key to `PROD`.
3. Fill in the following fields with your production credentials:
   * CleverTap Account Id: Your PRODUCTION Account ID
   * CleverTap Account Token: Your PRODUCTION Account Token
   * CleverTap Account Region (Optional): Region code for regional endpoints
   * CleverTap Proxy Domain (Optional): Custom proxy domain
   * CleverTap Spiky Proxy Domain (Optional): Spiky proxy domain

<Image align="center" border={true} caption="Configure the PROD Environment" src="https://files.readme.io/edd27f1799721f80ffdf795b75e034fc1a233c04a0fbe0be8f1a1c6f00237bd4-52b0090b299a60166716af9d8ac1153241cb7e622376cf0b16dfa3bda186cf3c-image.png" />

If the settings fail to load, reopen the settings window: _Assets_ > _CleverTap Settings_.
