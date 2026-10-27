
- **USB On-the-Go (OTG)** USB OTG is a specification that enables a compliant mobile device to have many different types of devices connect to it. For example, if your mobile device or tablet supports USB OTG, you could connect a communication device such as a keyboard or mouse and use that with your mobile device.
- **Recording microphone** For privacy reasons, you can disable the recording microphone on the organization’s devices.
- **GPS tagging** Evaluate the GPS tagging functionality on devices to ensure that the devices are not tagging the GPS coordinates of photos that are taken (if permitted). This can be used to track an employee’s location if they are taking photos and uploading them to social media.
- **Wi-Fi direct/ad hoc** You may want to monitor how the wireless networking feature is being used and control whether the user is connecting to wireless networks in infrastructure mode or if they are allowed to connect in ad hoc mode. With infrastructure mode, the administrator controls who can connect to the network and which security features are enabled.
- **Tethering** Tethering is when you connect your phones to a USB port on your laptop and use the Internet of your phone to get Internet access on your laptop. You may want to restrict the tethering features on the mobile devices to prevent users from using their Internet connections on their phone in this manner, as opening this connection could expose the laptop to an attack.
- **Hotspot** Companies may decide to disable the hotspot feature on the mobile device. The hotspot feature allows you to convert your mobile device into a wireless hotspot that others can connect to in order to gain Internet access through your mobile device. Opening a hotspot on your mobile device could leave you open to attack.
- **Payment methods** Monitor and control how users can make payments from mobile devices. With technologies like NFC, it is easy for users to complete transactions from mobile devices.

## Deployment Models

When an organization decides to have employees use mobile devices to perform job-related tasks, the organization can choose one of several different ways to integrate mobile devices into the workplace. The following outlines deployment models:

- **Bring your own device (BYOD)** The “bring your own device” model encourages users to connect to the corporate network with their personal mobile devices for work purposes. While the benefit is that the organization can avoid the cost of purchasing the mobile devices, you will need to be clear on the policy and whether the organization will push settings down to the devices. To learn more about the security concerns of BYOD, check out the section titled “BYOD Security Concerns,” later in this chapter.
- **Corporate-owned, personally enabled (COPE)** A corporate-owned, personally enabled (COPE) model can work better from a security standpoint than a BYOD model because it is hard for companies to control a device when they do not own the device. With COPE, the company supplies the device to the user, so it is managed by the IT department, but the company allows and promotes personal usage of the device as well.
- **Choose your own device (CYOD)** A choose your own device model involves the organization providing users with a list of approved devices and allowing each user to choose which device they would like to use.
- **Corporate-owned** With a corporate-owned device model, the company fully manages the devices and employees must follow company policy when using them.
- **Virtual desktop infrastructure (VDI)** Virtual desktop infrastructure is a model where the user uses a thin client to connect to their desktop environment running in a data center. With VDI, you can introduce the mobile device as the thin client so that the user can access their desktop environment from anywhere. The benefit is that the resources are not on the mobile device—it simply connects to a virtual desktop within the company.

> exam
> Watch
> For the Security+ exam, know the different deployment models for mobile devices, such as BYOD, COPE, and CYOD.

## Mobile Device Management Solutions

Because mobile devices are commonly used to perform business today, many technologies have come out to help better manage the security of these devices:

- **MicroSD Hardware Security Module (HSM)** A MicroSD Hardware Security Module (HSM) is a special hardware security module that comes in the form of a MicroSD card. This security module provides security services such as encryption, digital signatures, authentication, and key life cycle management.
- **MDM/Unified Endpoint Management (UEM)** Organizations can centrally manage all mobile devices and enforce policies on the mobile devices with Mobile Device Management (MDM) software. This capability can be extended with Unified Endpoint Management (UEM) software, which allows a company to manage all endpoint devices, including mobile devices, laptops, and desktop systems.
- **Mobile Application Management (MAM)** Mobile Application Management is when a company uses enterprise software to centrally manage the applications running on mobile devices. This includes delivering applications to the device, updating applications, and implementing application security from a central point.
- **SEAndroid** Security Enhancements Android is a project that adds security enhancements to the Android operating system. The security enhancements are designed to prevent data leakage by applications, prevent privilege escalation by apps, and protect the integrity of apps on the device and its data.

## Data Security

A huge part of securing systems is to ensure that the data on a system or device is protected from unauthorized individuals. You should protect data in a number of different ways:

- **File permissions** You can control who has access to what files in order to prevent unauthorized individuals from accessing the files under normal circumstances. The reason I say “normal circumstances” is that I have seen a file protected with permissions that were bypassed by booting from a live disc and accessing the data.
- **Encryption** To ensure that information is confidential and protected from unauthorized use, encrypt the data in storage. If you encrypt the data and an unauthorized individual gains access to the file, they will see only the encrypted text and be unable to decipher the information.
- **Data loss prevention (DLP)** Data loss prevention is software or hardware that is designed to analyze information leaving the organization to ensure that sensitive information is not being leaked. DLP can be implemented to monitor network traffic and to identify sensitive information being sent through instant messenger, e-mail, or protocols such as HTTP and FTP. DLP can also analyze data in storage to ensure sensitive information is not being stored in the wrong location.

## Windows File Permissions

In the Windows operating systems, you can secure the folders and files so that only certain users can access certain folders. For example, you want to make sure that only accountants can get access to the accounting folder.

There are two steps to securing folders on a Windows server: secure the folder with NTFS permissions and then set share permissions when you share the folder out to the network. Sharing the folder out to the network is the way you “publish” the folder to your Windows clients. Users can connect to shared folders only when connecting to the server. The following list summarizes NTFS permissions and sharing:

- **NTFS permissions** These can be applied only to NTFS partitions. If you are not using NTFS, you can convert to NTFS from FAT or FAT32 with the convert <drive_letter> /fs:ntfs command. Once you set the NTFS permissions, they will apply when the user accesses the folder either locally or from across the network.
- **Sharing** Sharing the folder is the way you publish the folder to the network clients. When you share the folder, you specify share permissions as well. Remember that when the NTFS permissions conflict with share folder permissions, the most restrictive permission wins. For example, if you give the NTFS permission of Modify and the share permission of Read, when users come through the share to access that folder, their permission will be Read because it is more restrictive.

![](img/3e3af33e2c753e920c09ecd40b5a457a_339_0.png)

**NTFS Permissions** You should be familiar with a number of NTFS permissions when it comes to securing folders; the popular NTFS permissions are listed in Table 7-1.

**Shared Folder Permissions** Once you secure the folders with NTFS permissions, you will need to share the folder to publish it to the network. When sharing folders, you will need to configure shared folder permissions. You should be familiar with a few shared folder permissions when it comes to securing folders that are shared on the network. Be aware that when a share permission is applied to a folder, it will be retained for all subfolders as well.

| NTFS Permission | Description |
|---|---|
| List Folder Contents | This permission is assigned to a folder; it allows a user to view a contents list for the folder but not necessarily to read the files in the folder. |
| Read | This is a folder and file permission that enables a user to open and read the contents of files. |
| Read and Execute | This is a folder and file permission that allows users to read the contents of files and to execute an executable. |
| Write | This is a file and folder permission that allows a user to modify the contents of a file (write to it) or to create a new file or folder within that folder. |
| Modify | This is a folder or file permission that includes all the permissions mentioned previously. Having the Modify permission allows a user to read, execute, delete, list folder contents, and write to the contents of the folder or file. |
| Full Control | This is a folder or file permission that gives a user all permissions possible. If you assign the Full Control permission, the user will be able not only to modify the contents of the file but also to change the permissions on the resource. |
