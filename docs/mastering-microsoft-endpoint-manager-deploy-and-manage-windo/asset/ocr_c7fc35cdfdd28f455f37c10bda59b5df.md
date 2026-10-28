# Endpoint protection

The endpoint protection workload includes the Windows Defender suite of anti-malware protection features:

+   - **Antivirus:** Antivirus policies help security admins focus on managing the settings for Microsoft Defender Antivirus, Microsoft Defender exclusions, and the Windows Security experience as a policy for Intune-managed devices.
+   - **Disk encryption:** Endpoint security disk encryption profiles focus on only the settings that are relevant for a device's built-in encryption method, such as BitLocker. This makes it possible for an IT admin to deploy and automatically encrypt end users' Windows devices.
+   - **Firewall:** Use the endpoint security firewall policy in Intune to configure a device's built-in firewall for devices that run Windows 10. Firewall policies are divided into two, the firewall policy itself and a firewall rule policy.
+   - **Endpoint detection and response:** When you integrate Microsoft Defender for Endpoint with Intune, use the endpoint security policies for **endpoint detection and response (EDR)** to manage the EDR settings and onboard devices for Microsoft Defender for Endpoint.
+   - **Attack surface reduction:** When Defender Antivirus is in use on your Windows 10 devices, use Intune endpoint security policies for attack surface reduction to manage those settings for your devices.

The attack surface reduction policy is divided into different policies:

+   A. App and browser isolation  
+   B. Device control  
+   C. Attack surface reduction rules  
+   D. Exploit protection  
+   E. Web protection for Microsoft Edge Legacy  
+   F. Application control

+   - **Account protection:** Account protection policies help you protect the identity and accounts of your users. The account protection policy is focused on settings for Windows Hello and Credential Guard, which is part of Windows identity and access management.

When you switch this workload, the Configuration Manager policies stay on the device until the Intune policies overwrite them. This behavior makes sure that the device still has protection policies during the transition.