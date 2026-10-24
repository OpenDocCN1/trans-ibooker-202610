
converts the information back into an electrical signal. This type of cabling provides tremendous capacity, offering potential signaling rates in excess of 200,000 Mbps. However, current access protocols still limit fiber-optic LAN speeds to 100 Mbps.

![](img/2459befd842d6fed299fabe6da3512d0_428_0.png)

**FIGURE 16.3** Transmitting Over Fiber-Optic Cable

Light moving through a fiber-optic cable does not *attenuate* (lose energy) as quickly as electrical signals moving along a copper conductor. Therefore, segment lengths between transmitters and receivers can be much longer when using fiber-optic cabling. In some fiber-optic applications, the maximum cable length can range up to 2 kilometers.

## A Fiber-Optic World Speed Record

Researchers from the United States and the Netherlands have broken a world speed record by using a single, multicored piece of glass fiber. They were able to push 255 terabits per second, roughly equivalent to 32 terabytes per second.

Because it cannot be tapped without physically breaking the conductor, fiber-optic cable also provides a much more secure data transmission medium than copper cable. Basically, light introduced into the cable at one end does not leave the cable except through the other end. In addition, fiber-optic cable electrically isolates the transmitter and receiver so that signal level matching normally needs to be performed between the two ends.

## TABLE 16.1 Bluetooth Parameters

| Class | Maximum Power | Operating Range |
|---|---|---|
| Class 1 | 100 mW (20 dBm) | 100 meters |
| Class 2 | 2.5 mW (4 dBm) | 10 meters |
| Class 3 | 1 mW (0 dBm) | 1 meter |

The Bluetooth specification implements Adaptive Frequency Hopping Spread Spectrum (AFHSS) in the license-free 2.4GHz range to provide security and avoid crowded frequency ranges. The Bluetooth protocol divides the 2.4 GHz frequency range into 79 different 1 MHz communication channels. The frequency-hopping mechanism changes channels up to 1,600 times per second. Lastly, the frequency hops channels in one of six predefined patterns. All this frequency hopping is done to lower the effects of other electronic interference.

The data transfer rate for Bluetooth version 1.1 and 1.2 devices is 723.1 Kbps and a big boost of 2.1 Mbps for Bluetooth 2.0 devices.

Bluetooth devices can be connected to only one device at a time; connecting to them will prevent them from connecting to other devices and showing up in inquiries until they disconnect from the other device. However, the standard also provides for constructing multipoint wireless networks using Bluetooth technologies.

Under the Bluetooth specification, up to eight devices can be grouped together to form a piconet. Any device can become the master device and assume control of the network by issuing a request broadcast. The other seven devices become slave devices until the master device releases its position.

The master device uses time division multiplexing to rapidly switch from one slave device to another around the network. In this manner, the Bluetooth network operates like a wireless USB network. Any device in the network can assume the master device role when it is available.

In the computer networking environment, the Bluetooth specification enables several Bluetooth peripheral devices to simultaneously communicate with a host device. In particular, Bluetooth is used with local host computers communicating with wireless input and output devices such as mice, keyboards, and printers.

Like Bluetooth, the ZigBee (IEEE 802.15.4) standard is a wireless, mesh-networked PAN protocol that provides for a 10-meter communication range with data transfer rates at 250 Kbps, as shown in Figure 16.5. The ZigBee standard has been embraced by the smart home automation and industrial controls communities, as well as several areas of the smart grid consortium. It is also being considered for use with personal biomedical sensors to provide secure, remote medical-data acquisition.

![](img/2459befd842d6fed299fabe6da3512d0_431_0.png)

Figure 16.5 ZigBee PAN

The IEEE 802.16 – WiMAX specification was established to provide guidelines for wider area wireless networking capabilities. WiMAX is a broadband wireless access standard designed to provide Internet access across large geographic areas such as cities, counties, and in some cases countries, as shown in Figure 16.6. It is also designed to provide interoperability with the 802.11 Wi-Fi standard.

![](img/2459befd842d6fed299fabe6da3512d0_431_1.png)

Figure 16.6 WiMAX

## Transmission Media Vulnerabilities

As with other components of the network, transmission media security must be considered at two levels: physical security and logical security. *Physical security* involves securing the physical medium, along with the communication equipment and physical ports that interconnect the networked equipment. If attackers can gain uninterrupted access to the transmission media, they can find a way to exploit it.

Securing physical media becomes challenging when it leaves the controllable area of a private facility. Within the facility, optical and metal communication media are relatively safe as a physical tapping of the media is required to extract information from it. The same cannot be said for wireless communications because they can be extracted from the air through a simple antenna.

However, when physical media leaves the confines of the private facility, the information they carry becomes vulnerable to interception and capture along their route or at the receiving port of the message.

### Securing Wireless Networks

While wireless networks are very popular due to their ease of installation, there are a number of security issues concerning using them to communicate personal or otherwise sensitive information. Transmissions from wireless network devices cannot simply be confined to the local environment of a residence or business.

Although the range of most wireless network devices is typically limited to a few hundred feet, RF signals can easily be intercepted even outside the vicinity of the stated security perimeter. Any unauthorized mobile terminal can accomplish this using an 802.11 receiver. Any intercepted transmissions, being unencrypted, are then easily read.

In order to minimize the risk of security compromise on a wireless LAN, the IEEE-802.11 wireless standard provides for several encryption options. An early security feature called Wired Equivalent Privacy (WEP) provides a 128-bit mathematical key encryption scheme for encrypting data transmissions and authenticating each computer on the network. Enabling the WEP function adds some security for data being transmitted. However, at this time, WEP no longer provides the assurance it had years ago.

You will also need to enter the WEP key value (password), either in the form of hexadecimal number string or as an ASCII character string. Working with the ASCII option is easier for most people. Record this string for use with the network’s client computers. Each client computer will need to have the key

## Hands-On Exercises

### Objectives

The purpose of this lab is to examine some of the more advanced security configuration settings of your router. Logs will be examined and turned on. SSID broadcasting will be disabled. Next, antennas will have their power lowered and turned off entirely. Finally, Universal Plug and Play will be disabled.

### Resources

- PC with Windows 10 physically connected to your SOHO router
- SOHO router

### Procedure

- 1. Open the Microsoft Edge browser.
- 2. Type in the IP address of your gateway in the address bar of your browser. Remember, you can find this by going to the command line and issuing IPCONFIG. (See Steps 7 through 12 in Chapter 15’s hands-on exercise for additional assistance.)
- 3. At the login screen (see Figure 16.7), provide your administrator credentials.

![](img/2459befd842d6fed299fabe6da3512d0_434_0.png)

**FIGURE 16.7** Enter your credentials.
