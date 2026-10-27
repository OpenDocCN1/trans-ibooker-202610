
when the operating system loads. Most systems today will ask for a username and password (which is better than just a password), but the point is that you want to make sure no systems log on automatically when booted.

Consider password-protecting other resources such as routers, switches, and even printers. Most people do not think about using passwords for their mobile devices such as laptops or phones—but these devices contain sensitive company information, so they also should be password-protected when the system boots up.

Not only should the system require a password when booted, but you should also make sure you are following good password practices on these devices. All passwords should be a minimum of eight characters long and should use a mix of lowercase and uppercase characters, numbers, and symbols. Also be sure that users are not writing down their passwords, because it is easy for someone to find the paper the password was written on.

## Registry Hardening

When it comes to hardening a system, you can also harden the registry. The registry is a database of all the settings for your Windows system. Usually, you configure the system by going through the Control Panel, but you could also use regedit.exe in Windows to make changes through the registry. If you make an incorrect change in the registry, it is possible that you could cause the system to no longer function properly, so it is important to only go into the registry if you are familiar with it.

As far as hardening the registry, the biggest thing you should look at is ensuring that only administrators have the permission to make changes to critical areas of the registry. For example, malicious software may add itself to the Run portion of the registry, which is where programs that automatically start are located. You could modify the permissions on the Run area of the registry to ensure that only administrators can modify this portion of the registry.

While discussing the registry, I should also mention that making changes to the registry can be useful in locking down the system, as there are many settings in the registry that are not found in the Control Panel. Here are some useful points about using the registry:

- Regedit.exe is the tool we use to modify the registry.
- You can right-click an area of the registry and choose the permissions command to modify the permissions on that area of the registry.
- You can export settings from the registry to a .reg file with the regedit.exe /E command.
- You can import a registry file of settings with the regedit.exe <filename.reg> command. This is a great way to ensure custom settings in the registry are easily deployed to multiple systems.

## Disk Encryption

When hardening systems, you should also look to harden the drives on the system. You could do this by setting permissions on the files and folders found on the hard drive, but it has been found that attackers can bypass the permissions if they can boot to an alternate operating system and then access the drive. For that reason you should look to encrypt the hard drive. If you encrypt the contents of the drive, an unauthorized person cannot access the files unless they have the decryption key—even if they boot to an alternate operating system.

There are different drive encryption technologies you should be aware of:

- **Encrypting File System (EFS)**  The Encrypting File System (EFS) is a security feature in Windows you can place on files located on a drive formatted for NTFS. You can go to the properties of a file and then, on the General tab, click the Advanced button. You can then set the attribute to encrypt the file. By default, only the person who encrypts the file can decrypt the file. Note that this is a way to encrypt individual files and not the entire hard drive.
- **BitLocker**  BitLocker is a drive encryption feature within Windows that allows you to encrypt the entire drive. This is recommended on mobile devices such as laptops so that if the laptop is lost or stolen, you have the confidence of knowing the hard drive is encrypted.
- **BitLocker To Go**  BitLocker To Go is a feature that allows you to encrypt USB drives. Because these are portable drives that are easily lost or stolen, you should encrypt them with BitLocker To Go.

## System Hardening Procedures

Now that you have a general idea of what system hardening is, let’s look at some of the popular tools security administrators are using to harden networks and systems. In this section you will learn about network hardening and some of the software tools used.

### Network Security Hardening

The first aspect of network hardening you need to consider is updating the firmware on all networking devices. Network devices, although hardware, are like computers in the sense that they are run by software. The software that runs the network devices is known as *firmware* and is stored in flash memory known as EEPROM, which stands for *electrically* erasable programmable read-only memory. You can normally go to the manufacturer's web site for the device and download a revised, updated version of the firmware. This version of the firmware will contain fixes to any bugs at the time it was created.

After you update the firmware, be sure you have configured a password for the device so that only authorized individuals can manage it. Most devices today, such as routers and switches, support a web management interface, which you should password-protect and configure to use HTTPS if supported.

When looking at hardening the network, ensure you are using the most secure devices possible. For example, use switches instead of hubs because switches have a filtering feature that ensures the switch sends the traffic only to the port on which the destination system resides. This means that it is hard for someone to put a sniffer on the network and capture the traffic because the traffic never reaches the device doing the packet sniffing. Back when hubs were popular network devices, the packets would be sent to all ports on the hub, and anyone with a sniffer installed on a system could view all the network traffic.

### Port Security

Most network switches today support a feature known as port security that you can use to limit which systems can connect to a port on the switch by listing specific MAC addresses with the port. Port security is also known as MAC limiting because you are limiting by MAC addresses which systems can connect to a port.

The following code shows how you can configure port security on a Cisco switch. This example limits which systems can connect to port 1 on the switch by configuring a MAC address for that switch port:

```
NY-SW1>enable
NY-SW1#config term
NY-SW1(config)#interface f0/1
NY-SW1(config-if)#switchport mode access
NY-SW1(config-if)#switchport port-security
NY-SW1(config-if)#switchport port-security mac-address 0000.1111.2222
NY-SW1(config-if)#switchport port-security maximum 1
NY-SW1(config-if)#switchport port-security violation shutdown
```

Let’s review the preceding code. The first three lines are used to navigate to Fast Ethernet port 1 on the switch. Then the port is placed in access mode so that the workstation can connect to the port, and the port security feature is enabled with the switchport port-security command. The lines that follow are used to configure a MAC address (0000.1111.2222) for the port and then to configure a maximum of one address for that port so that the switch does not learn any additional MAC addresses for it. Finally, you configure the port to disable (shutdown) if there is an address violation, meaning if someone with a different MAC address connects to the port, you want to disable the port so that network access is not allowed.

### MAC Limiting and Filtering

MAC filtering is a popular access control method implemented with many different devices such as network switches and wireless access points. With a wireless access point, you can implement MAC filtering to control which MAC addresses can connect to the wireless network. Note that this is also referred to as *MAC limiting* because you are limiting which devices can access the wireless network by MAC address.

MAC filtering on a switch has a slightly different meaning in that it allows you to restrict which systems can send data to other systems on the network. Filtering on a switch is usually configured along with the port security feature and allows you to restrict which ports on the switch are allowed to be “source ports” for data sent to a particular system.

### Disable Unused Interfaces (Ports)

Not only should you limit which systems can connect to which ports on the switch, but you should also disable any unused ports on the switch. This will stop someone from walking into your office, plugging into a port with their laptop, and gaining access to your network. The following code demonstrates how to navigate to port 2 on a switch and then to disable the port with the **shutdown** command:

```
NY-SW1>enable
NY-SW1#config term
NY-SW1(config)#interface f0/2
NY-SW1(config-if)#shutdown
```

You can also disable a group of ports at one time by selecting a range of ports with the **interface range** command on the Cisco switch. After the ports are selected, you can use the **shutdown** command to disable those ports, as shown next:

```
NY-SW1>enable
NY-SW1#config term
NY-SW1(config)#interface range f0/4-8
NY-SW1(config-if-range)#shutdown
```
