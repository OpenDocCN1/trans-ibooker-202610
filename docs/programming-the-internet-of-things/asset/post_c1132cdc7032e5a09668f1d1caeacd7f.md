
communications paradigms through a single protocol standard.

## Part IV – Connecting to the Cloud

Finally, at the ‘top’ of the integration stack, you’ll learn how to connect all your IoT device infrastructure to the cloud by using your gateway application to serve as the go-between for your cloud functionality and all your devices.

This section covers basic cloud connectivity principles, and lightly covers the various cloud services that can store, analyze, and manage your IoT environment. In each case, you’ll build the same simple cloud application across each platform.

- Chapter 10 considers various IoT-centric cloud services that are built on open source platforms that can either be re-hosted in your own environment or accessed via a free or paid tier.
- Chapter 11 provides an overview of various commercial cloud services that have IoT-specific capabilities. As there are many books and tutorials available for these platforms, I won’t go into detail on building anything specific; this chapter will simply serve as a review of these capabilities.
- Chapter 12 examines a few simple IoT use cases that I’ve found particularly helpful in preparing my Connected Devices course at Northeastern University. Specifically, I’ll cover the overall problem statement, expected outcome, design approach, and target solution for a home environmental monitoring system, pet monitoring system, and hydroponic garden management system.

In each chapter, I’ll provide a brief introduction to the topic along with some helpful background material, which will include some pertinent definitions that should be useful in understanding the concepts I’ll discuss. I’ll also summarize why each of these topics is important and what you can expect to learn. Each chapter ends with some hands-on implementation exercises to help you take the information presented and apply it to your own application.

My hope is that this approach will allow you to understand, and create, an integrated IoT system - end to end. It’s also designed so you can skip to the chapters that are most relevant for you as more of a reference guide, so you can skip the exercises if you’re not interested in coding a solution, although I do recommend you give them a try! If you do decide to delve into each lab module, please keep in mind that they are cumulative in that the work you do for each chapter will generally be used – or referenced – in a later chapter.

## Some Background on the IoT

It may be helpful to look at just what enabled these ecosystems, and so here’s a brief summary of how we got to this point. Computing took a big step forward with the invention of the transistor in the 1950’s, followed in the 1960’s by Gordon Moore’s paper describing the doubling of transistors packed in the same physical space (later updated in the 1970’s)².

With modern computing came modern networking, and the beginnings of the Internet with the invention of the ARPAnet in 1969³. This led to new ways to chunk, or packetize, data using the Network Control Protocol (NCP) and Transmission Control Protocol (TCP) via the Internet Protocol (IP) and leveraging existing wired infrastructure in the 1970’s. This was useful for industry, allowing electrical industrial automation to move down the path of centralized management of distributed, connected systems. Supervisory Control and Data Acquisition (SCADA) systems emerged from their proprietary roots, and Programmable Logic Controllers (PLC’s) were developed to take advantage of TCP/IP networking and related equipment standards.⁴

Eventually, we arrived at the 1980’s with the introduction of User Datagram Protocol (UDP)⁵ and the birth of what many of us experienced as the early modern Internet - the World Wide Web (WWW) - invented in the late 1980’s by Tim Berners-Lee⁶.

I’m sure you’ve noticed a common theme: A problem is followed by a technology innovation (often proprietary in nature) to address the challenge, which then becomes standardized (or is superseded by one or more standards), leading to wide adoption and further innovation.

This brings us to the era of the IoT, where in the late 1980’s and early 1990’s we begin to see the first connected devices emerge, including the demonstration of an Internet-connected

toaster, demonstrated by John Romkey and Simon Hackett at 1990’s Interop show⁷. Shortly thereafter, a web camera was set up to monitor the coffee pot near the Trojan Room at the Computer Lab at the University of Cambridge in 1991⁸. Because who wants to make a trip to the coffee machine and find the coffee pot empty? 

More devices followed, of course, and I’d guess even more were built and connected as experiments in college labs, dorms, homes, apartments, and businesses. All the while, computing and networking continued to become more inexpensive, powerful, and - of course - smaller. In 1999, Kevin Ashton presented the “Internet of Things” at Proctor & Gamble⁹, which is widely believed to be the first coining of the phrase.

Fast forward to 2005, and the Interactive Design Institute in Italy gives us the inexpensive and designed-for-novices Arduino Single Board Computer (SBC)¹⁰, opening the door for more people to start building their own sensing and automation systems. Add easily accessible storage and the ability to analyze data through services reachable from anywhere on the Internet, and we have the underpinnings of an IoT ecosystem; that is, a lot of different things that may be individually unique, but can be connected to each other in such a way as to serve a larger purpose.

In this sense, I believe it’s best not to view the IoT as a bunch of things that connect the physical world and the Internet to do useful work. The essence of the IoT is heterogeneity: lots of

dissimilarity in terms of device types, features and capabilities, and purposes as well as implementation approaches, supported protocols, security, and management techniques.

## Complexity Redefined

So, what *exactly* is the ‘Internet of Things’? It’s a complex set of technology ecosystems that connect the physical world to the Internet using a variety of *edge computing devices* and *cloud computing services*.

For the purposes of this book, edge computing devices represent the embedded electronics, small computers and software applications that either interact directly with the physical world through sensors and actuators or provide a gateway, or bridge, for those devices and applications to connect to the Internet. Cloud computing services represent the computing systems, software applications, data storage, and other computing services that live within one or more data centers and are always accessible via the Internet.

To be sure, this is an oversimplification of two complex, and often overloaded, technical terms. My intent is to provide a relatively simple categorization of the key computing resources for which to discuss the IoT and the integrated system you’ll build as you work through the exercises in this book.

With that said, and for the remainder of the book, I’ll focus on the IoT in terms of architecture and purpose; that is, how an

IoT system is designed, and the outcome the design is expected to achieve.

The first step is to map these two technical terms of edge computing devices and cloud computing services into *architectural tiers*, where edge computing devices are part of the *edge tier*, and cloud computing services are part of the *cloud tier*. This provides both physical and logical separation of the key functionality of an IoT system, meaning, for example, that all sensing and actuation will take place in the edge tier, and all long-term storage and complex analytics will take place in the cloud tier.

This separation of tiers represents a high-level depiction of the systems architecture, or plan, that I’ll use going forward to describe the composition of the various physical and logical components comprising an integrated IoT system.

### NOTE

The Industrial Internet Consortium (IIC) has published a variety of useful documents on a subset of the Internet of Things, called the Industrial Internet of Things (IIoT). One such publication, the Industrial Internet Reference Architecture (located here https://www.iiconsortium.org/IIRA.htm), discusses a framework and common vocabulary for IIoT systems, and has heavily influenced my thinking on the topic of IoT architecture.¹¹
