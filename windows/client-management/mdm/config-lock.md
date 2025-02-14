---
title: Secured-Core Configuration Lock
description: A Secured-Core PC (SCPC) feature that prevents configuration drift from Secured-Core PC features (shown below) caused by unintentional misconfiguration. 
manager: dansimp
keywords: mdm,management,administrator,config lock
ms.author: v-lsaldanha
ms.topic: article
ms.prod: w11
ms.technology: windows
author: lovina-saldanha
ms.date: 03/14/2022
---

# Secured-Core PC Configuration Lock 

**Applies to**

-   Windows 11

In an enterprise organization, IT administrators enforce policies on their corporate devices to keep the devices in a compliant state and protect the OS by preventing users from changing configurations and creating config drift. Config drift occurs when users with local admin rights change settings and put the device out of sync with security policies. Devices in a non-compliant state can be vulnerable until the next sync and configuration reset with the MDM. Windows 11 with Config Lock enables IT administrators to prevent config drift and keep the OS configuration in the desired state. With config lock, the OS monitors the registry keys that configure each feature and when it detects a drift, reverts to the IT-desired state in seconds.

Secured-Core Configuration Lock (Config Lock) is a new [Secured-Core PC (SCPC)](/windows-hardware/design/device-experiences/oem-highly-secure) feature that prevents configuration drift from Secured-Core PC features caused by unintentional misconfiguration. In short, it ensures a device intended to be a Secured-Core PC remains a Secured-Core PC.

To summarize, Config Lock:

- Enables IT to “lock” Secured-Core PC features when managed through MDM
- Detects drift remediates within seconds
- DOES NOT prevent malicious attacks

## Configuration Flow

After a Secured-Core PC reaches the desktop, Config Lock will prevent configuration drift by detecting if the device is a Secured-Core PC or not. When the device isn't a Secured-Core PC, the lock won't apply. If the device is a Secured-Core PC, config lock will lock the policies listed under [List of locked policies](#list-of-locked-policies).

## System Requirements

Config Lock will be available for all Windows Professional and Enterprise Editions running on [Secured-Core PCs](/windows-hardware/design/device-experiences/oem-highly-secure).  

## Enabling Config Lock using Microsoft Intune

Config Lock isn't enabled by default (or turned on by the OS during boot). Rather, an IT Admin must intentionally turn it on.
 
The steps to turn on Config Lock using Microsoft Endpoint Manager (Microsoft Intune) are as follows:

1. Ensure that the device to turn on Config Lock is enrolled in Microsoft Intune.
1. From the Microsoft Intune portal main page, select **Devices** > **Configuration Profiles** > **Create a profile**.
1. Select the following and press **Create**:
    - **Platform**: Windows 10 and later
    - **Profile type**: Templates
    - **Template name**: Custom

    :::image type="content" source="images/configlock-mem-createprofile.png" alt-text="In Configuration profiles, the Create a profile page is showing, with the Platform set to Windows 10 and later, and a Profile Type of Templates":::

1. Name your profile.
1. When you reach the Configuration Settings step, select “Add” and add the following information:
    - **OMA-URI**: ./Vendor/MSFT/DMClient/Provider/MS%20DM%20Server/ConfigLock/Lock
    - **Data type**: Integer
    - **Value**: 1 </br>
    To turn off Config Lock, change the value to 0.

    :::image type="content" source="images/configlock-mem-editrow.png" alt-text="In the Configuration settings step, the Edit Row page is shown with a Name of Config Lock, a Description of Turn on Config Lock and the OMA-URI set as above, along with a Data type of Integer set to a Value of 1":::

1. Select the devices to turn on Config Lock. If you're using a test tenant, you can select “+ Add all devices”.
1. You'll not need to set any applicability rules for test purposes.
1. Review the Configuration and select “Create” if everything is correct.
1. After the device syncs with the Microsoft Intune server, you can confirm if the Config Lock was successfully enabled.

    :::image type="content" source="images/configlock-mem-dev.png" alt-text="The Profile assignment status dashboard when viewing the Config Lock device configuration profile, showing one device has succeeded in having this profile applied":::

    :::image type="content" source="images/configlock-mem-devstatus.png" alt-text="The Device Status for the Config Lock Device Configuration Profile, showing one device with a Deployment Status as Succeeded and two with Pending":::

## Configuring Secured-Core PC features

Config Lock is designed to ensure that a Secured-Core PC isn't unintentionally misconfigured.  IT Admins retain the ability to change (enable/disable) SCPC features (for example Firmware protection) via Group Policies and/or mobile device management (MDM) tools, such as Microsoft Intune.

:::image type="content" source="images/configlock-mem-firmwareprotect.png" alt-text="The Defender Firmware protection setting, with a description of Windows Defender System Guard protects your device from compromised firmware. The setting is set to Off":::
 
## FAQ

**Can an IT admins disable Config Lock ?** </br>
	Yes. IT admins can use MDM to turn off Config Lock.</br>

### List of locked policies

|**CSPs**     |
|-----|
|[BitLocker ](bitlocker-csp.md)      |
|[PassportForWork](passportforwork-csp.md)       |
|[WindowsDefenderApplicationGuard](windowsdefenderapplicationguard-csp.md)       |
|[ApplicationControl](applicationcontrol-csp.md) 


|**MDM policies**     | **Supported by Group Policy** |
|-----|-----|
|[DataProtection/AllowDirectMemoryAccess](policy-csp-dataprotection.md)      | No |
|[DataProtection/LegacySelectiveWipeID](policy-csp-dataprotection.md)      | No |
|[DeviceGuard/ConfigureSystemGuardLaunch](policy-csp-deviceguard.md)      | Yes |
|[DeviceGuard/EnableVirtualizationBasedSecurity](policy-csp-deviceguard.md)      | Yes |
|[DeviceGuard/LsaCfgFlags](policy-csp-deviceguard.md)      | Yes |
|[DeviceGuard/RequirePlatformSecurityFeatures](policy-csp-deviceguard.md)      | Yes |
|[DeviceInstallation/AllowInstallationOfMatchingDeviceIDs](policy-csp-deviceinstallation.md)      | Yes |
|[DeviceInstallation/AllowInstallationOfMatchingDeviceInstanceIDs](policy-csp-deviceinstallation.md)      | Yes |
|[DeviceInstallation/AllowInstallationOfMatchingDeviceSetupClasses](policy-csp-deviceinstallation.md) | Yes |
|[DeviceInstallation/PreventDeviceMetadataFromNetwork](policy-csp-deviceinstallation.md) | Yes |
|[DeviceInstallation/PreventInstallationOfDevicesNotDescribedByOtherPolicySettings](policy-csp-deviceinstallation.md) | Yes |
|[DeviceInstallation/PreventInstallationOfMatchingDeviceIDs](policy-csp-deviceinstallation.md) | Yes |
|[DeviceInstallation/PreventInstallationOfMatchingDeviceInstanceIDs](policy-csp-deviceinstallation.md) | Yes |
|[DeviceInstallation/PreventInstallationOfMatchingDeviceSetupClasses](policy-csp-deviceinstallation.md) | Yes |
|[DmaGuard/DeviceEnumerationPolicy](policy-csp-dmaguard.md) | Yes |
|[WindowsDefenderSecurityCenter/CompanyName](policy-csp-windowsdefendersecuritycenter.md) | Yes |
|[WindowsDefenderSecurityCenter/DisableAccountProtectionUI](policy-csp-windowsdefendersecuritycenter.md) | Yes |
|[WindowsDefenderSecurityCenter/DisableAppBrowserUI](policy-csp-windowsdefendersecuritycenter.md) | Yes |
|[WindowsDefenderSecurityCenter/DisableClearTpmButton](policy-csp-windowsdefendersecuritycenter.md) | Yes |
|[WindowsDefenderSecurityCenter/DisableDeviceSecurityUI](policy-csp-windowsdefendersecuritycenter.md) | Yes |
|[WindowsDefenderSecurityCenter/DisableEnhancedNotifications](policy-csp-windowsdefendersecuritycenter.md) | Yes |
|[WindowsDefenderSecurityCenter/DisableFamilyUI](policy-csp-windowsdefendersecuritycenter.md) | Yes |
|[WindowsDefenderSecurityCenter/DisableHealthUI](policy-csp-windowsdefendersecuritycenter.md) | Yes |
|[WindowsDefenderSecurityCenter/DisableNetworkUI](policy-csp-windowsdefendersecuritycenter.md) | Yes |
|[WindowsDefenderSecurityCenter/DisableNotifications](policy-csp-windowsdefendersecuritycenter.md) | Yes |
|[WindowsDefenderSecurityCenter/DisableTpmFirmwareUpdateWarning](policy-csp-windowsdefendersecuritycenter.md)| Yes |
|[WindowsDefenderSecurityCenter/DisableVirusUI](policy-csp-windowsdefendersecuritycenter.md) | Yes |
|[WindowsDefenderSecurityCenter/DisallowExploitProtectionOverride](policy-csp-windowsdefendersecuritycenter.md) | Yes |
|[WindowsDefenderSecurityCenter/Email](policy-csp-windowsdefendersecuritycenter.md) | Yes |
|[WindowsDefenderSecurityCenter/EnableCustomizedToasts](policy-csp-windowsdefendersecuritycenter.md) | Yes |
|[WindowsDefenderSecurityCenter/EnableInAppCustomization](policy-csp-windowsdefendersecuritycenter.md) | Yes |
|[WindowsDefenderSecurityCenter/HideRansomwareDataRecovery](policy-csp-windowsdefendersecuritycenter.md) | Yes |
|[WindowsDefenderSecurityCenter/HideSecureBoot](policy-csp-windowsdefendersecuritycenter.md) | Yes |
|[WindowsDefenderSecurityCenter/HideTPMTroubleshooting](policy-csp-windowsdefendersecuritycenter.md) | Yes |
|[WindowsDefenderSecurityCenter/HideWindowsSecurityNotificationAreaControl](policy-csp-windowsdefendersecuritycenter.md) | Yes |
|[WindowsDefenderSecurityCenter/Phone](policy-csp-windowsdefendersecuritycenter.md) | Yes |
|[WindowsDefenderSecurityCenter/URL](policy-csp-windowsdefendersecuritycenter.md) | Yes |
|[SmartScreen/EnableAppInstallControl](policy-csp-smartscreen.md)| Yes |
|[SmartScreen/EnableSmartScreenInShell](policy-csp-smartscreen.md) | Yes |
|[SmartScreen/PreventOverrideForFilesInShell](policy-csp-smartscreen.md) | Yes |
