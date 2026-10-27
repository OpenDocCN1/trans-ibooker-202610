+   - **Service pack** A service pack provides all updates for a product, including patches and security hotfixes, from the time the product was released up to the time of the service pack. If you install a service pack, you will not need to install each patch individually, because the service pack includes all updates up to that point. You will need to install patches and security fixes that come out after the service pack.

To patch the system, you can run the Windows Update program from within the Start menu or use centralized patch management software such as Windows Server Update Services (WSUS) to deploy patches to a number of systems at one time. It is important that as a security professional you evaluate how patches are being deployed to the systems and ensure that the strategy is keeping the systems up to date with security fixes.

As part of the patch deployment strategy, ensure that network administrators are not applying patches to systems without first testing the patches. Administrators should have a group of test systems they deploy the patches to first to ensure the patches do not cause business applications to stop working. Once a patch has been verified against the test systems, it should be deployed to a small group of production systems. After verifying the patch has not caused issues with the small group of production systems, you can then deploy the patch to a larger group.

![](bbox=[0.113, 0.474, 0.927, 0.6])

Another consideration with patching systems is that you should apply any third-party updates for additional software that has been installed on the system. When you apply updates to the operating system, software from other third-party companies will not be patched, so it is important to research how to apply updates to the third-party software running on the system.

You should also look to enable auto-update features within the operating system and in the applications running on the system. If you enable auto-updates, you lower the risk of forgetting to apply updates at a later time—they will be applied automatically.

# Password Protection

A final practice you should incorporate into your system-hardening procedure is placing password protection features on your asset. From a system point of view, this means you will ensure you have password-protected the CMOS setup program so that unauthorized changes to the CMOS cannot occur. Also, make sure the system prompts for a password.