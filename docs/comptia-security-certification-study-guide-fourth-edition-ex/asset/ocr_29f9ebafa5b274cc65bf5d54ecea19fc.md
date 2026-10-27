when the operating system loads. Most systems today will ask for a username and password (which is better than just a password), but the point is that you want to make sure no systems log on automatically when booted.

Consider password-protecting other resources such as routers, switches, and even printers. Most people do not think about using passwords for their mobile devices such as laptops or phones—but these devices contain sensitive company information, so they also should be password-protected when the system boots up.

Not only should the system require a password when booted, but you should also make sure you are following good password practices on these devices. All passwords should be a minimum of eight characters long and should use a mix of lowercase and uppercase characters, numbers, and symbols. Also be sure that users are not writing down their passwords, because it is easy for someone to find the paper the password was written on.

# Registry Hardening

When it comes to hardening a system, you can also harden the registry. The registry is a database of all the settings for your Windows system. Usually, you configure the system by going through the Control Panel, but you could also use regedit.exe in Windows to make changes through the registry. If you make an incorrect change in the registry, it is possible that you could cause the system to no longer function properly, so it is important to only go into the registry if you are familiar with it.

As far as hardening the registry, the biggest thing you should look at is ensuring that only administrators have the permission to make changes to critical areas of the registry. For example, malicious software may add itself to the Run portion of the registry, which is where programs that automatically start are located. You could modify the permissions on the Run area of the registry to ensure that only administrators can modify this portion of the registry.

While discussing the registry, I should also mention that making changes to the registry can be useful in locking down the system, as there are many settings in the registry that are not found in the Control Panel. Here are some useful points about using the registry:

+   - Regedit.exe is the tool we use to modify the registry.
+   - You can right-click an area of the registry and choose the permissions command to modify the permissions on that area of the registry.
+   - You can export settings from the registry to a .reg file with the regedit.exe /E command.
+   - You can import a registry file of settings with the regedit.exe <filename.reg> command. This is a great way to ensure custom settings in the registry are easily deployed to multiple systems.