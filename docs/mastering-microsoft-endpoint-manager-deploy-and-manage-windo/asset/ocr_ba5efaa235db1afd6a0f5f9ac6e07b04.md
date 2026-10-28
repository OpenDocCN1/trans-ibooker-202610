The settings in the settings catalog are generated directly from Windows CSPs, and the settings experience in the catalog is dynamically generated based on the type of setting and its metadata, such as tooltips. The settings catalog continues to add new Windows settings and reduces the need to deploy custom policies from Microsoft Intune (OMA-URI-based). One of the new things in the settings catalog is that if a setting is not in your policy, then it will be considered not configured. Editing a policy later on and removing a setting from an existing settings catalog policy will not only remove that setting from the policy, but it will also remove the previously set enforcement from assigned devices on the next device check-in.

There are thousands of settings to choose from in the settings catalog, including settings that are not in any other policy type in Microsoft Intune and have not been available before in the console. You can also configure Administrative Templates in the settings catalog. As Windows adds or exposes more settings to MDM providers, these settings are quickly added to the Microsoft Intune settings catalog to be configured.

To create a profile with the settings catalog, you open the Microsoft Endpoint Manager admin center:

+   1. Go to Home > Devices > Windows > Configuration profiles and apply the following:

+   - Platform: Select Windows 10 and later

+   - Profile type: Select Settings catalog (preview):

![](bbox=[0.15, 0.56, 0.82, 0.75])

Figure 10.11 – Create a profile

+   2. As part of the profile creation, you need to fill out the wizard:

+   - Name: Enter a descriptive name for the profile, such as Settings Catalog Policy.