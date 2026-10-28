
exercise, you’ll be able to

- Understand how change management relates to the COVID-19 pandemic
- Understand how a lack of a change management plan could be harmful, especially during the COVID-19 pandemic
- Understand what should go into a change management plan, especially for pandemic situations

## Lab Materials and Setup

The materials you need for this lab are

- The *Principles of Computer Security: CompTIA Security+ and Beyond* textbook
- A web browser with an Internet connection

## Let’s Do This!

You’re going to watch a couple of very interesting webinars relating the COVID-19 pandemic to change management. Watch carefully, and take notes. After watching, you will write a summary of what you learned from each webinar.

1b

Step 1 The first webinar comes from Panorama Consulting Group, which is described at www.panorama-consulting.com as follows:

100% independent of enterprise resource planning [ERP] software vendor affiliation, Panorama offers a phased and integrated approach to strategy alignment and execution, enabling each client to achieve their unique vision. We offer the flexibility of either a top-down strategic approach, or a bottom-up tactical approach to our clients’ projects, depending on each client’s unique business transformation objectives.

This video shows change management tips to keep employees positive and productive in spite of significant organizational and global changes during

## Lab Analysis

- 1. How is business continuity different in a COVID-19 world?
- 2. How is disaster recovery different in a COVID-19 world?
- 3. How is change management different in a COVID-19 world?

## Key Term Quiz

Use the terms from the list to complete the sentences that follow.

- business continuity
- change management
- disaster recovery

- 1. Proper usage of __________ helps an organization succeed and achieve milestones, outcomes, and goals when things deviate.
- 2. To ensure that mission/business processes will be sustained during and after a significant disruption, organizations must have a plan for __________.
- 3. To guide an enterprise response to a major loss of enterprise capability or damage to its facilities, organizations turn to a plan for __________.

## Chapter 12
Wireless Security and Mobile Devices

## Lab Exercises

- 12.01 Wireless Network and Device Detection
- 12.02 Monitor Mode Sniffing
- 12.03 Cracking WPA2 Passwords and Deauthenticating Clients with Wifite

Lab Analysis

Key Term Quiz

When you walk down the street or hallway, do you hear other people talking? Do you try to listen in to any of those conversations? Do those people know you are listening to them? Is that illegal?

I’ve personally heard people in hallways actually shouting on a cell phone, “Well, you need to sign in first. You can use my account. My username is x and my password is y” (where x and y are the actual credentials). I’ve even heard people, very loudly, say their Social Security numbers on cell phone calls. I am continually floored each time I hear people almost yelling their sensitive information into their phones—within earshot of others!

Similar questions about legality have been tossed around for many years regarding Wi-Fi traffic. The latest news to come out regarding that goes all the way back to 2010, 2012, and 2014.

- This 5/17/10 article explains how Google admits that its Street View cars collected actual data from unencrypted Wi-Fi networks by accident, instead of just collecting intended network names and router MAC addresses:
https://www.infosecurity-magazine.com/news/google-says-street-view-cars-collected-wifi-data-9499/

- This 7/1/11 article explains how a federal judge denied Google’s request to dismiss the class-action lawsuit against them, claiming they violated the Federal Wiretap Act by collecting unencrypted Wi-Fi data:
https://www.infosecurity-magazine.com/news/federal-judge-rejects-googles-motion-to-dismiss/

- In this 7/1/11 article, “Ms. Smith” explains more about the decision to allow a lawsuit against Google to continue:
https://www.csoonline.com/article/2220115/sniffing-open-wifi-may-be-wiretapping-judge-tells-google.html

- In this 4/16/12 article, David Goldman explains that Google was fined $25,000 for obstructing a federal regulator’s investigation into the interception of unencrypted Wi-Fi traffic:
https://money.cnn.com/2012/04/16/technology/google-fcc/index.htm

- In this 4/27/12 article, David Kravets explains that the Justice Department cleared Google of claims that it illegally intercepted unencrypted Wi-Fi traffic:
https://www.wired.com/2012/04/doj-google-streetview/

- In this 9/7/12 article, Timothy B. Lee explains how a federal judge handed down a ruling that if you intercept unencrypted Wi-Fi traffic, that does not constitute a violation of wiretapping. Innovatio IP Ventures wanted a preliminary ruling to allow them to intercept Wi-Fi traffic to prove that certain businesses offering public Wi-Fi infringed on its patents. The ruling is the opposite of a 2011 pronouncement against Google, related to Google’s Street View cars capturing of Wi-Fi traffic from all around the United States.
https://arstechnica.com/tech-policy/2012/09/sniffing-open-wifi-networks-is-not-wiretapping-judge-says/

- In this 9/17/12 article related to the same case, Kent Lawson explains that there is no expectation of privacy or legal claims for anyone not

cybersecurity.

## Learning Objectives

In this lab exercise, you’ll explore wireless network signals. At the end of this lab exercise, you’ll be able to

- Understand how easily wireless networks can be detected
- Understand how much information about wireless networks can easily be detected
- Understand how easily wireless devices can be detected
- Understand how much information about wireless devices can easily be detected

## Lab Materials and Setup

The materials you need for this lab are

- The *Principles of Computer Security: CompTIA Security+ and Beyond* textbook
- A Windows 10 host machine with an Internet connection
- A wireless network interface card (NIC) connected to a Wi-Fi network
- A wireless access point (WAP), also known as just an access point (AP)

## Let’s Do This!

You’re about to download and install two programs. Make sure you’re connected to a Wi-Fi network before continuing.

1b, 1c. Feel free to crop or redact any portion of the screenshots for privacy.

1d

Step 1 On your Windows 10 machine, download and install NetSpot. The following description is from www.netspotapp.com:

NetSpot is the only professional app for wireless site surveys, Wi-Fi analysis, and troubleshooting on Mac OS X and Windows. It’s a FREE Wi-Fi analyzer. No need to be a network expert to improve your home or office Wi-Fi today! All you need is your MacBook running Mac OS X 10.10+ or any laptop with Windows 7/8/10 on board and NetSpot which works over any 802.11 network.

- a. Go to www.netspotapp.com.
- Click the Get NetSpot button.
- In the NetSpot FREE edition section, click the Download Now button.
- The executable will download to your Downloads folder. Click it to run the executable and install NetSpot.
- Click the blue Install Now button to begin the installation.
- Click the Launch button when the installation is complete.
- Click the Continue button on the bottom left to keep using the free version.

- b. While the operating system can give you basic and limited information about wireless networks you’re in range of, with the Discover tab selected, you’ll see information in the following categories: SSID, BSSID, Graph, Signal, %, Min, Max, Average, Level, Band, Channel, Width, Vendor, Security, Mode, and Last Seen. Figure 12-1 shows NetSpot in action.

![](img/6e921d997751f0a0f87e5170ac9657a1_368_0.png)

# FIGURE 12-1 NetSpot detecting wireless signals

SSID (service set identifier) is the name of the network. BSSID (basic service set identifier) is the AP’s MAC address.

NetSpot provides a great page at www.netspotapp.com/help/terms-definitions/ that contains “All the clever words used in NetSpot and Wi-Fi related science explained.” Check it out.

- c. If you put a check in the color-coded checkbox in the first column for some or all rows, you can get information comparing those SSIDs to the others by clicking Details on the bottom bar (or by simply double-clicking a row without putting a check in any box). As shown in Figure 12-2, you’ll see a new window with the following tabs: Signal (5 min, 30 min, 60 min), Tabular Data, Channels 2.4 GHz, and Channels 5 GHz. The Signal, Channels 2.4 GHz, and Channels 5 GHz tabs will show aggregate information for the SSIDs selected. You can add and remove checks to dynamically change the color-coded output.
