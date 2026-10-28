
2. Create a new policy and enter a custom name for your Azure MFA policy. Once you've done this, you can go through all the layers in the wizard to activate the correct settings to trigger MFA's secondary authentication:

![](img/a3e9f26159d3739e434fe676b2987c1d_129_0.png)

Figure 4.40 – Entering the name of your policy

3. Select the list of users and/or AAD groups you wish to filter and activate MFA for. I am going to pick cloud PC users.

> Important Note
> When selecting All users, make sure to not lock yourself out! This policy will affect all of your users. We recommend applying a policy to a small set of users first to verify that it behaves as expected.

4. Open the cloud apps or **Actions** menu and select **cloud PC** as the cloud app. Make sure that the cloud app ID for **cloud PC** is `0af06dc6-e4b5-4f28-818e-e78e62d137a5`.

![](img/a3e9f26159d3739e434fe676b2987c1d_130_0.png)

Figure 4.41 – Selecting Windows 365

5. Click on **Conditions** to start the creation process. This will allow you to control the user access layer based on signals from conditions such as risk, device platform, location, client apps, or device state.

6. Select the settings that align best with your security requirements. I'm going to use the device marked as **Compliant** to make sure users with a compliant device can connect to a cloud PC without entering MFA.

> **Important Note**
> 
> All other accounts will be enforced by the MFA prompt.

7. Move to the **Grant** menu. Select the settings that align best with your security requirements.

8. Make sure that you select **Require multi-factor authentication**:

![](img/a3e9f26159d3739e434fe676b2987c1d_130_1.png)

Figure 4.42 – Require multi-factor authentication

9. Go to the Session menu. This part is optional; however, the sign-in frequency makes it possible to enforce MFA prompts for as little as 1 hour after your existing token expires.

> Important Note
> Especially for Bring-Your-Own-Device (BYOD) scenarios, this could be beneficial. Existing sessions on your cloud PC will remain active within the time you configure this setting.

10. The persistent browser feature makes it possible to remain signed in after closing and reopening a browser window.

11. Enable and create the policy by clicking on ON and then Create.

12. Make sure that the Enable policy setting is set to Off by default.

13. Confirm that the policy has been added with the On state.

Next time you log into your cloud PC, you'll be prompted for MFA!

## Security baselines for a cloud PC

Using a virtualized desktop in the Microsoft cloud requires a different security baseline than for physical Windows PCs. Therefore, we created a new baseline optimized for cloud PCs in the profile catalog. Make sure that you select the cloud PC security baseline for the best performance and security settings for your business:

Home > Endpoint security

Endpoint security | Security baselines

![](img/a3e9f26159d3739e434fe676b2987c1d_131_0.png)

Figure 4.43 – Selecting a cloud PC security baseline

![](img/a3e9f26159d3739e434fe676b2987c1d_133_0.png)

Figure 4.45 – Unzipping the Prep Tool

3. Download the latest Remote Desktop client from `http://aka.ms/wvdclient`.
4. Save the **Microsoft Installer (MSI)** in the same folder where the Prep Tool is.
5. Create the Win32 `.Intunewin` package.
6. Open PowerShell and change the directory to the Intune Prep Tool's location; for example, `C:\Microsoft-Win32-Content-Prep-Tool-master`:

```
cd "C:\Microsoft-Win32-Content-Prep-Tool-master"
```

7. Run `\IntuneWinAppUtil.exe` and fill in the following requirements:
   - Specify the source folder as `C:\Microsoft-Win32-Content-Prep-Tool-master`.
   - Specify the setup file as `RemoteDesktop_1.2.1755.0_x64.msi`.
   - Specify the output folder as `C:\Microsoft-Win32-Content-Prep-Tool-master`.
   - **Do you want to specify the catalog folder (Y/N)?** N.

You can customize the folders as you wish, as shown here:

![](img/a3e9f26159d3739e434fe676b2987c1d_133_1.png)

Figure 4.46 – Running `\IntuneWinAppUtil.exe`

If everything runs correctly, the following creation wizard will start:

![](img/a3e9f26159d3739e434fe676b2987c1d_134_0.png)

Figure 4.47 – Running successfully

8. If everything runs successfully, you will see the `.intunewin` file listed in the folder. Now, we can switch to Microsoft Endpoint Manager:

![](img/a3e9f26159d3739e434fe676b2987c1d_134_1.png)

Figure 4.48 – The Intune win32 package has been added

9. Go to `http://endpoint.microsoft.com/` to continue this configuration process in the Microsoft Endpoint Manager admin center.

10. Go to **Apps | Windows apps | Add**:

![](img/a3e9f26159d3739e434fe676b2987c1d_135_0.png)

Figure 4.49 – Adding a new Windows app

11. Select **Windows app (Win32)**.

12. Click on **Select app package file**.

13. Browse and select the `.Intunewin` package file we created earlier, which includes the Remote Desktop client (MSRDC).

14. Click on **OK**:

![](img/a3e9f26159d3739e434fe676b2987c1d_135_1.png)

Figure 4.50 – Selecting a package file

15. On the next screen, you can customize the name of the app.

*Optional:* Set **Company Portal** to **Yes** if you wish to make installing the enrollment optional for users:

![](img/a3e9f26159d3739e434fe676b2987c1d_136_0.png)

16. Click on **Next**.

17. Use the following settings:

```
msiexec /i "RemoteDesktop_1.2.1755.0_x64.msi" /qn
ALLUSERS=2 MSIINSTALLPERUSER=1
```

Figure 4.52 – Defining the install command

18. Provide the minimum app requirements:

Figure 4.53 – Adding requirements

19. Create a detection rule based on the manual configuration.

20. Select MSI to perform the detection based on the MSI product code to see whether the app is already installed on the endpoint.

21. Click on OK:

![](img/a3e9f26159d3739e434fe676b2987c1d_138_0.png)

Figure 4.54 – Detection rule

22. We can skip Dependencies. Then, click Next:

![](img/a3e9f26159d3739e434fe676b2987c1d_138_1.png)

Figure 4.55 – Skipping Dependencies

23. Assign the application to an AAD group, or all devices, to enforce deployment to your physical Windows PCs/endpoints.

> Important Note
> The client should not be installed inside your cloud PC VM, but on your physical endpoint. If your physical endpoint is not MEM managed, please update/install the Remote Desktop client manually from here: http://aka.ms/CPCclient.

24. Click on Select and then Next.

25. Edit the assignment if you want to enforce the enrollment or update process.

*Optional:* There's a new preview feature available, called **Supersedence**, in Microsoft Endpoint Manager. This new feature enables you to update and replace existing Win32 apps with newer versions of the same app. You can learn more by going to `https://docs.microsoft.com/en-us/mem/intune/apps/apps-win32-supersedence`.

![](img/a3e9f26159d3739e434fe676b2987c1d_139_0.png)

Figure 4.56 – Selecting supersedence

26. Click on **Create** to push the Remote Desktop client to your physical Windows PCs:

![](img/a3e9f26159d3739e434fe676b2987c1d_139_1.png)

Figure 4.57 – Pushing the Remote Desktop client to your PC

The application will now be pushed to all the endpoints of users who are part of the AAD group filter.

You will see that the Remote Desktop app has been added as a recent app.

The Remote Desktop Windows application has been added to the Start menu:

![](img/a3e9f26159d3739e434fe676b2987c1d_140_0.png)

Figure 4.58 – App added to the start menu

27. After registration, you will be able to start your cloud PCs directly from the Tile and Start menus:

![](img/a3e9f26159d3739e434fe676b2987c1d_140_1.png)

Figure 4.59 – The Let's get started configuration screen

28. Log in with your AAD user account and pass the necessary MFA prompts:

![](img/a3e9f26159d3739e434fe676b2987c1d_141_0.png)

Figure 4.60 – Logging in with your account

You can also pin them directly to the taskbar!

## Cloud PCs – device-based filtering

You may encounter scenarios where you already have a configured Microsoft Endpoint Manager tenant to manage your physical endpoints. Existing policies, application delivery rules, or other configuration items could be filtered to **All Devices**. This could cause conflict as there may be cases where the setting should only apply to your physical environment.
