
After you have disabled all unused ports, a time may come when a port needs to be enabled because a new employee has been hired and needs a connection to the network. To enable a port on a Cisco switch, you navigate to the port and then use the no shutdown command:

```
NY-SW1>enable
NY-SW1#config term
NY-SW1(config)#interface f0/4
NY-SW1(config-if)#no shutdown
```

> exam Watch For the Security+ exam, remember that any unused ports on the switch should be disabled as part of the process of hardening the network.

## 802.1X

A popular approach for hardening the network is to ensure that anyone who connects to the network supplies valid credentials before the network connection is allowed. This is different from a normal operating system logon in the sense that when you log on to a system, the system typically already has network access. The 802.1X standard is an IEEE standard for controlling access to the network (both wired and wireless) and is typically referred to as port-based access control protection. With 802.1X, you are not given access to the network until you authenticate to the 802.1X environment.

The way 802.1X works is that you have a client such as a desktop system, laptop, or network device that wishes to connect to a network switch or wireless access point. The network switch or wireless device is known as the authenticator and needs to have the connecting devices authenticated before granting network access. The switch or wireless access point sends an authentication request to a central authentication server running a protocol such as RADIUS or DIAMETER (see Figure 6-5). These are common authentication servers for different types of network environments.

> exam Watch Remember for the Security+ exam that 802.1X is a common authentication protocol to control who gains access to the physical network resources, such as connecting to a switch or wireless access point.

## FIGURE 6-5

802.1X involves a network device sending an authentication request to a central authentication server.

![](img/3e3af33e2c753e920c09ecd40b5a457a_281_0.png)

## Secure Management Protocols

As part of hardening network devices such as routers and switches, you should ensure that only secure management protocols are running on the device. For example, you should disable Telnet as a remote management protocol because it does not encrypt communication between the management system and the device. This includes authentication traffic not being encrypted, which means that someone can capture the Telnet traffic and see your Telnet username and password. As a hardening technique, you would ensure Telnet is disabled but enable SSH as the remote management protocol. SSH encrypts all communication between the SSH client (the administrator’s management system) and the network device—including authentication traffic.

## EXERCISE 6-2

## Hardening a Network Switch

In this exercise you will harden a network switch by disabling port 3 on the switch because no system is planned for that port. You will also configure port security on port 10 so that only your MAC address can be used on that port.

- 1. Ensure you have a Cisco switch powered on. Connect to the console port on the switch from your station’s serial port.

- 2. To disable port 3 on the switch, type the following commands:

```
NY-SW1>enable
NY-SW1#config term
NY-SW1(config)#interface f0/3
NY-SW1(config-if)#shutdown
```

- 3. To configure port security on port 10, type the following command and use your MAC address where indicated (use the format of 1111.2222.3333):

```
NY-SW1(config)#interface f0/10
NY-SW1(config-if)#switchport mode access
NY-SW1(config-if)#switchport port-security
NY-SW1(config-if)#switchport port-security mac-address <MAC>
NY-SW1(config-if)#switchport port-security maximum 1
NY-SW1(config-if)#switchport port-security violation shutdown
```

- 4. Connect a workstation other than the one with your MAC address to port 10, and try to ping another system on the network. Were you successful? ____________

## Rogue Machine Detection

Part of the job of monitoring your network environment is monitoring the network for rogue machines and devices. A rogue machine is a machine connected to the network that does not belong there. There are a number of different reasons why an individual may connect a system or device to the network:

- **A rogue system** Someone may connect a system that runs a packet sniffer to the network with the intent of capturing confidential information such as passwords transmitted across the network.
- **A rogue device** An example of a rogue device is a wireless router being connected to the network. An employee may connect a wireless router to the network to allow them to roam throughout the office with a mobile device or laptop and use the company network for Internet access.

Although hardening the network infrastructure and using features such as port security on a network switch should reduce the chances of having a rogue system or device connected to your network, it is important to ensure you monitor and track which systems and devices are connected to the network. Once you identify an unauthorized device connected to the network, you should disable that port on the switch immediately and then investigate the rogue system or device.

## Tools for System Hardening

Now that you are familiar with some popular techniques for controlling who has access to the network, let’s look at some of the common tools you can use to harden a system. You will need to be familiar with these concepts for the Security+ certification exam, so be sure to try them.

## Group Policies

The first important tool for hardening a Windows system is known as *group policy*, a core feature of Windows that allows the network administrator to enable and disable different features in Windows—for example, group membership, services running, password policies, and desktop restrictions such as whether the user should have the **run** command available.

Group policies can be configured on the local system (the system the administrator is configuring) or can be centrally configured in the Active Directory domain, which means the settings will apply to a number of systems and users every 90 minutes, on next user logon, or on system restart.

To configure group policies on a single system, click the Start button and then type **mmc** (for Microsoft Management Console). Once in the MMC, choose File | Add/Remove Snap-In. When you have a list of snap-ins in front of you in the Add or Remove Snap-Ins dialog box, choose the Group Policy Object Editor and click Add to move it to the Selected Snap-Ins list. Click the Finish button to accept that you are configuring the local computer and then click OK.

You are now ready to configure the local group policies on the system (see Figure 6-6). Group policies are divided into two categories—computer settings and user settings. The computer settings apply to the machine no matter who is logged on, whereas the user settings apply to the user account. To summarize the difference between the two, the *user* settings typically contain ways to restrict the desktop settings, whereas the *computer* settings allow you to harden the system with password policies, by disabling services, and with software restrictions (what software is allowed to run on the system), to name a few.

![](img/3e3af33e2c753e920c09ecd40b5a457a_283_0.png)

The following is a quick description of some of the common policies found in the Computer Configuration section of group policies:

- **Windows Settings | Scripts (Startup/Shutdown)** In this policy, you can configure a startup script for when the computer first boots up or a shutdown script for when the computer is shut down.
- **Security Settings | Account Policies** This policy section allows you to configure policies related to user accounts such as account lockout and password policies.
- **Security Settings | Local Policies** A very important policy section that relates to system hardening. In this section you can configure user rights on the system, auditing, and other security settings such as creating a logon banner.
- **Security Settings | Windows Firewall with Advanced Security** This policy allows you to configure the new firewall feature built into Windows systems after Windows XP.
- **Security Settings | Software Restriction Policies** This policy allows you to configure what software is allowed to run on the system.
- **Security Settings | Advanced Audit Policy Configuration** This policy allows you more control over the auditing of the system and the types of events you want to audit.

The following outlines some of the popular policies found in the User Configuration settings of group policies:

- **Windows Settings | Scripts (Logon/Logoff)** This policy is used to configure scripts that execute when a user logs on or off.
- **Windows Settings | Internet Explorer Maintenance** This policy is used to configure settings in IE such as a favorites list or default home page.
- **Administrative Templates | Control Panel** This policy allows you to enable or disable features of the Control Panel in Windows. The benefit here is that by disabling components in the Control Panel, you can keep users from making system changes.
- **Administrative Templates | Desktop** This policy setting allows you to control what icons should appear on the desktop such as My Computer, Internet Explorer, and My Documents.
- **Administrative Templates | Start Menu and Taskbar** This policy setting allows you to control what items can appear in the Start menu such as the Run command and the Search command.

If you want to modify the policies for all systems on the network, you can configure a group policy at the domain level. You can also configure a group policy on a specific organizational unit (OU) in Active Directory so that the policy only applies to the computers or users within a specific OU. For example, you may want to ensure Tom is a local administrator of
