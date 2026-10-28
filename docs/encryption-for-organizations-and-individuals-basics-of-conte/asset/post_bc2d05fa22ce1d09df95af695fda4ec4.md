
- **Timing SCAs** examine the delays in cryptographic operations. Different algorithms may have recognizable characteristics which can be exploited to compromise a cryptosystem. However, malicious actors attempting this type of attack usually need detailed information on the system such as the make of the CPU/processor and the overall system design paradigm being used.
- Timing SCAs can be foiled, in select cases, by keeping the hardware internals of a system hidden. The technique is known as *security through obscurity*, which has been a widely rejected approach since the nineteenth century. Skilled malicious actors can discover the makeup of any hardware rather trivially in no time.
- *Blinding* is a technique which can protect an asymmetric cryptographic system (i.e., a public-key scheme such as RSA or ElGamal) against timing SCAs. In this context blinding works to add random elements into an algorithm’s input which are not entirely known by any single party. There are several ways to implement cryptography-related blinding and some are more efficient than others.

## Cyberthreats Not Specific to Cryptography

We’ll now explore some common threats not specifically designed against encrypted systems. As mighty as they seem, encrypted systems aren’t immune to any of these attacks; when a nonmalicious user has entered the right password and booted up a computer, online malware is as serious a threat as on a completely nonencrypted system.

## Man-in-the-Browser (MITB)

This type of threat takes over a web browser using its potential vulnerabilities. It’s a remote Trojan horse able to modify the contents of web pages from the user’s perspective. Worst of all, an MITB is capable of interfering with online bank transfers and the amounts of currency they process. In fact, this type of attack is considered one of the worst threats to secure online banking, since it’s often completely invisible to the user.

## Boy-in-the-Browser (BITB)

A smaller-scale version of the man-in-the-browser attack also exists, called the *boy-in-the-browser attack*. BITBs target visitors to specific malicious websites. A type of malware is forcefully installed to change a user’s computer’s network traffic to become a part of a man-in-the-middle attack in the online realm. After the damage has been done, the program might delete itself, making its detection nearly impossible.

## Botnet

Consisting of a large number of connected devices and functioning online, a *botnet* is in essence a centrally controlled “army” of said devices usually used for malicious purposes. A bot refers to an infected computer or smartphone. Botnets can yield a lot of digital leverage and can consists of millions of bots; the user may not even be aware that his or her device has been compromised. For one, email spammers love their botnets. Also, they are used to distribute Trojan horses and other malware online.

> Infected systems that are a part of a botnet are usually called *zombie computers.*

## Distributed Denial-of-Service (DDoS) Attack

One of the most common and frustrating attacks an organization can face is an attack called *distributed denial of service*. This online offense is executed using large botnets which exhaust a website of its resources, resulting in the whole site crashing down and offering very little apart from an error message to its visitors. This can be devastating for critical businesses dealing in online banking, health care, and others. In 2012 six major US banks, including *Bank of America*, experienced issues thanks to a large-scale distributed denial-of-service attack. A smaller-scale variety of this attack, *denial of service (DoS)*, exists; these may target smaller organizations from time to time. Attacks of this type often originate from a smaller number of sources.

## Phishing Attack

Whenever a malignant piece of software or website poses as a legitimate party, it’s called *phishing*. A very popular type of cyber-scam, this kind of attack is used to “fish” for usernames, passwords, credit card numbers, and other sensitive data from unsuspecting users thinking they’re dealing with genuine businesses.

General-purpose mass phishing aside, there are several more unique techniques for this attack. *Whaling* refers to targeting high-profile targets, such as politicians or corporate executives. This type of approach can take the form of a well-written subpoena or other legal document and often results in sensitive data or a hefty sum of money sliding into the hands of a malicious actor. Any critical data obtained from a high-profile individual is naturally worth quite a tad in the black market. Whaling is sometimes also referred to as *business email compromise (BEC)*.

One of the biggest examples of whale phishing was the 2016 incident when the then-presidential candidate *Hillary Clinton’s* campaign head *John Podesta* was tricked to hand over his email account password by malicious actors. The phishing email simply told Podesta his Gmail account had been hacked (by actors in Ukraine, no less) and that he needed to change the password – by clicking a detrimental link.

*Spear phishing* is simply the malicious act of using a personalized approach for gaining information or financial gain. The social media revolution fueled this type of attack due to the considerable amount of individuals who release a large amount of data about their lives on a daily basis. A spear phisher may pose as a person’s colleague, friend, or even spouse to extract money or other resources.

Possibly the hardest to detect, *clone phishing* is the act of intercepting legitimate communications, such as email, and injecting it with a malicious attachment, message, or link. An email may seem to originate from a perfectly legitimate party but was in fact tampered with by a malicious third party. Usually the bad sender’s email address is either a variation of the original sender’s address or a straight-up nonsensical one; always pay attention to these details.

Bad actors on their way into the world of phishing benefit greatly from the so-called phishing kits. These are basically more or less authentic-looking duplicates of websites by well-known businesses, such as Facebook, Apple, or Microsoft. Combined with large mailing lists, perhaps obtained from the seedier parts of the Internet, these kits offer many potential victims for wannabe cybercriminals. Faked websites using phishing kits tend to last a very short period of time online. However, in a span of 24 hours or even less, these sites can inflict a great deal of harm to a plethora of individuals and organizations; new ones are created on a daily, if not on an hourly, basis.

Lastly, there’s yet another type of phishing attack known as *pharming*, which deals with the faking of *Domain Name Service (DNS)* entries. This is the technology which translates *Internet Protocol (IP)* addresses (e.g., *84.234.64.207*) preferred by electronic devices into something more pleasant to the human eye (e.g., *robertciesla.com*). Pharming transports a user, who entered a perfectly legitimate website address, into a malicious site dedicated for harvesting personal information. In some cases, these imitation sites are relatively easy to spot due to grammatical mistakes or other design flaws. A pharming site can also be visually perfectly similar when compared to its legitimate counterpart.

The various types of phishing attacks aren’t limited to fake emails and websites. Cybercriminals use the full range of tools at their disposal, such as social media platforms, instant messaging services, and even faked online advertisements. Stay alert and watch out for even the smallest of discrepancies in tone, design, and sender identity in all of your online communications.

## Policeware

Many governments are known to spy on their citizens. There’s a specialized type of software for this purpose which is often referred to as *policeware*. Once installed on a citizen’s/suspect’s computer, it can be very hard to detect and remove; many makers of anti-malware products may or may not have co-operated with law enforcement to guarantee this.

Perhaps the first large-scale example of policeware was *Carnivore* (later renamed *DCS1000*), devised by the FBI in the late 1990s and used primarily for email surveillance. It consisted of a Windows-based computer physically set up at a place of interest, usually at an *Internet Service Provider (ISP)* server room.
