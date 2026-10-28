Set **Deployment mode** to **Self-Deploying (preview)** to onboard the device as a userless device:

Home > Devices > Windows > Windows Autopilot deployment profiles >

# Create profile

Windows PC

Basics   Out-of-box experience (OOBE)   Scope tags   Assignments   Review + create

Configure the out-of-box experience for your Autopilot devices

Deployment mode *   Self-Deploying (preview)

Join to Azure AD as   Azure AD joined

Microsoft Software License Terms   Show   Hide

Important information about hiding license terms

Privacy settings   Show   Hide

The default value for diagnostic data collection has changed for devices running Windows 10, version 1903 and later. Learn more

Hide change account options   Show   Hide

User account type   Administrator   Standard

Language (Region)   Operating system default

Automatically configure keyboard   No   Yes

Apply device name template   No   Yes

Figure 7.33 — Windows Autopilot profile OOBE

# Self-Deploying (preview)

Self-deploying mode does not presently associate a user with the device (since no user ID or password is specified as part of the process).

Self-deploying mode uses a device's TPM 2.0 hardware to authenticate the device in an organization's AAD tenant. Therefore, devices without TPM 2.0 can't be used with this mode. Devices must also support TPM device attestation. This Windows Autopilot scenario will require a device with a physical TPM, so a virtual device will not work.

To achieve a 100% zero-touch onboarding experience, an Ethernet connection is required; otherwise, the device will prompt you for a region and keyboard layout.