
## Communication Considerations

Embedded systems and IoT devices will use different methods of communication to send the data collected to the manufacturer’s system. When determining the risk to IoT devices used by the company, don’t forget to research the communication technology utilized and vulnerabilities in the communication channel. The following are communication technologies used by these devices:

- **5G** Embedded systems and IoT devices can use cellular networks to transmit collected information to the manufacturer, with the current standard being the 5th-generation (5G) cellular network.
- **Narrowband** Devices may use narrowband radio communication channels for short-range communication. For example, RFID technology and remote keyless entry with vehicles typically use narrowband.
- **Baseband radio** The baseband radio is a communication technology that uses radio waves to send a signal between two points.
- **Subscriber identity module (SIM) cards** A SIM card is a small computer chip placed in a cell phone that stores information needed to connect to the cellular network. You can also store personal data such as messages and contacts on the SIM card.
- **Zigbee** Zigbee is a communication protocol used to create a personal area network (PAN) between devices such as home automation systems or medical device equipment.

## Constraints

The following are some potential limitations you may experience when working with embedded systems or IoT devices:

- **Power** IoT devices will need to use power to create a strong enough signal to send data. Manufacturers need to ensure that the components are supplied with enough power for stable functionality of the component.
- **Compute** The component will need to have the computing power to process the information collected.
- **Network** The network needs to be efficient and reliable to be able to transmit collected data.
- **Crypto** To secure the communication, the technology will use some form of cryptographic algorithm to encrypt communications. For the exam, remember to watch for solutions using weak encryption algorithms such as DES and 3DES. You will learn more about encryption in Chapters 12 and 13.
- **Inability to patch** Another key point with IoT devices to remember for the exam that’s a huge limitation or vulnerability is a device that does not give the ability to apply updates (patch the system). If a device does not provide a way to apply patches, that means vulnerabilities found within the device will always exist.
- **Authentication** Look at the authentication protocols supported by the device and ensure that if you can choose an authentication protocol, you use the most secure protocol possible (one that supports encrypted authentication traffic).
- **Range** The range of the technology can also be a limitation. For example, Bluetooth 4.2 has a limited range of 60 meters, while Zigbee has a maximum range of 100 meters.
- **Cost** The cost of the embedded technology could increase the cost of the component for the customer.
- **Implied trust** The technology may follow the rule of implied trust, where it trusts all communication.

## Certification Summary

In this chapter you learned about some of the most common types of threats against systems today. You have learned about physical threats to assets, such as device theft, and about malicious software and hardware threats. The following are some key points to remember about system security threats:

- Most companies focus their IT strategy on technology; however, they should not forget about physical security threats such as snooping and theft of hardware.
- A clean desk policy and ensuring that employees are shredding unneeded documents will help prevent snooping.
- Ensure that you password-protect any mobile devices and also encrypt the data on those devices in case they are lost or stolen.
- Be familiar with the different types of malicious software, such as viruses, spyware, and adware.
- Be sure to protect your system from malicious software by having virus protection software installed and the virus definitions kept up to date. You can use operating system commands such as **netstat**, **tasklist**, and **taskkill** to monitor what is running on the system.
- Ensure that users are encrypting any data stored on removable drives in case a removable drive is lost or stolen—this includes USB thumb drives. Also be sure to control which drives can be connected to corporate systems; you do not want a thumb drive that was infected with a virus from an employee’s home computer to enter your network.
- When you get new devices such as smart phones in the office, be sure to research the features of these devices and disable what is not being used, such as Bluetooth. Also, research the vulnerabilities of these particular devices.

Understanding threats against a system is an important skill to have as a security professional and will help you answer related questions on the exam. Always have a suspicious mind when it comes to security, and with any new technology, ask yourself, “What are the threats against this?”

## Two-Minute Drill

### Security Concerns with Vulnerabilities

- Watch for weak configuration of software or hardware devices that could make the system vulnerable to an attack. Examples of weak configuration are no permissions controlling actions that are authorized, weak encryption (such as the use of DES or 3DES), and insecure protocol usage (such as HTTP instead of HTTPS).
- Remember to modify the configuration from the system defaults, as hackers know the defaults of systems they attack.
- Be sure to apply patches to the system on a regular basis. This includes firmware updates, OS updates, and any application updates for software you are running.
- You should disable USB ports on systems or implement DLP to prevent data exfiltration.

### Identifying Physical Threats

- Ensure that employees shred sensitive documents when disposing of them to help prevent against snooping techniques such as dumpster diving.
- Implement a clean desk policy, specifying that sensitive documents are not allowed to be left in the open.
- Be sure employees are thoroughly educated on job tasks to help prevent human errors such as the accidental deletion of files or the destruction of computer equipment due to ESD.
- Encrypt and secure devices with passwords in order to protect company data that resides on a device in case it is lost or stolen.

### Looking at Malicious Software

- Privilege escalation is when a user is able to get administrative access to a system by exploiting a vulnerability on the system.
- A logic bomb is a type of virus that is triggered by a specific event such as a date.
- A worm virus is a self-replicating virus and replicates through flash drives, e-mail, or across the network in order to infect systems.
- A Trojan virus is a virus that you install thinking the program is a legitimate program, but instead it gives the hacker access to the system (typically by opening a port on your computer).
- You can use operating system commands such as **netstat**, **tasklist**, and **taskkill** to help troubleshoot systems that have been infected with malicious software. You may also look to the Microsoft Malicious Software Removal Tool for removing malware from an infected system.
- Spyware is software that collects information on your surfing habits, whereas adware is malware that loads advertisements on the screen in a pop-up window.
- Spam messages are mass-mailed unsolicited e-mails advertising a product or service.
- A rootkit is a piece of software installed by a hacker that grants them administrative access to the system later. Different types are application-level, library-level, kernel-level, and virtualized rootkits.
- A keylogger is a piece of software or hardware that captures keystrokes and stores them in a file for the hacker to review later.

### Threats Against Hardware

- You should configure the BIOS settings on all workstations and servers so that the systems will not boot from a bootable drive such as a USB drive or DVD drive that contains a bootable operating system.
- Be sure to disable any unused ports on a system, such as the USB ports, to prevent employees from taking sensitive data home with them, or even from introducing a virus to the system via a USB thumb drive.
- Be sure to investigate any known vulnerabilities with mobile phones and to disable unnecessary features on the phone.
- Bluesnarfing is the exploiting of a Bluetooth device by copying data from the device, whereas bluejacking is the sending of unsolicited messages to a Bluetooth device.
- Limit the use of removable media in the workplace in order to reduce the likelihood of intellectual theft. If you are going to use removable media, have the data on the drive encrypted in case the media is lost or stolen.
