
That same adversary later finds an Internet Explorer 11 code execution vulnerability and uses it in a series of attacks. However, after three months, defenders find and patch the vulnerability. The adversary had less time to get ROI out of that vulnerability, requiring him to go back to the drawing board and find a new one. This requires additional resource allocation for a smaller window of effectiveness.

## Exploitability

A vulnerability is just a crack in the armor. It takes an actual exploit to take advantage of that crack. The exploitability process is all about finding a method to trigger the vulnerability and turn that into actual control of program execution. Much like vulnerability hunting, this phase may have its own specialists or occur in conjunction with other exploit phases. This is a topic unto itself, addressed well in the book [*Hacking, The Art of Exploitation*](https://www.nostarch.com/hacking2.htm) by Jon Erickson (No Starch Press, 2008).

After an exploit is crafted, the adversary must make it reliable. This can be complicated because exploits don’t always work, given things like language packs and specific defenses such as Microsoft’s Enhanced Mitigation Experience Toolkit (EMET) or Address Space Layout Randomization (ASLR) for Linux. Further, exploits that crash target code or systems will draw attention.

An exploit, though, simply opens the door and gives the adversary a method to access the target (or at least an intermediate target). For the next step, the adversary will need an implant.

## Implant Development

Generally, the goal of an exploit includes delivering some sort of payload for the adversary to then use to further their goals (such as data exfiltration). The implant will allow the adversary to maintain access to the exploited system without having to continually exploit the device, which can be noisy, and if the system is patched, it would remove the possibility of exploitation at all. As a result, implant development follows many of the same processes as traditional software development, with an emphasis on stealth (to avoid detection) and capabilities (to allow adversaries to achieve their objectives). Thus, if an adversary wants to be able to listen to conversations within range of a compromised system, the implant needs the ability to activate the microphone, record what it hears, and transmit the resulting audio files, all without triggering suspicion from the user or any security software that’s running.

There are two primary types of implants. The first is a beaconing implant that calls out to a command-and-control server and will receive commands to be carried out on the target system. The second is an implant that does not beacon, but waits to receive a command and then begins to communicate with a command-and-control server. Implant development is often determined by the network topology and device type. Sometimes a previously developed implant can be used, but in other situations an adversary will need to develop something specifically for the network that is being targeted.

Although many computer network operations still lean heavily on the need for an adversary to keep persistence and install capabilities with implants, a growing number of actors seek to achieve their objectives without installing any implants. The compromise of Hillary Clinton campaign chairman John Podestas email was conducted without ever deploying an implant at all, just by stealing his password. In many ways, this style of attack is more difficult for investigators because without an implant, there’s one fewer artifact to analyze. Implantless attacks are another case where understanding the adversaries’ goals will help contextualize their techniques.

## Testing

Both exploits and implants then go through extensive testing as a part of the Weaponization phase. Much like software development, testing could be little more than a spot check, or it could mean extensive testing conducted by separate quality assurance teams. For malicious code, the testing phase focuses on two aspects: *function* and *detectability*.

The functionality aspect is much like any other software development project; the testing team needs to ensure that the software does what it’s designed to do. If it’s meant to steal files, the implant must be able to read from the filesystem on the target host; find the correct group of files; usually bundle, encrypt, and compress them; and then exfiltrate them to a system the adversary controls. This may seem easy, but there are a wide variety of variables that the development team may not always be able to control, and thus need to be tested for.

The detectability aspect is unlike anything seen in normal software development. Testing teams will attempt to verify that their software is undetectable by security tools they might see in their target environment, such as antivirus or other endpoint software. This ties directly into the functionality aspect as many heuristics-based security systems look for certain behaviors that the malicious code may need to achieve its objective, such as setting registry keys to maintain persistence. These detectability requirements may be based on assumptions, or for especially hard targets, based on information gathered during reconnaissance.

## Infrastructure development

While not strictly a part of the Weaponization phase, infrastructure development is another key preparation task an adversary needs to complete before the attack. Most attacks rely on pieces of infrastructure to support the malicious code deployed onto a victim’s machines. Command-and-control servers are needed to direct attack actions. Exfiltration points are needed to upload and then retrieve stolen data from. Hot points are needed for adversaries to pivot through to obfuscate their real location if their other infrastructure gets compromised. Adversaries need a wide variety of infrastructure to execute the various phases of their operation:

### Certificates

First of all, it’s important to differentiate the two most common types of certificates used in intrusions.

*   **Code signing certificates**, provided by operating system builders like Apple and Microsoft, are necessary to run new applications without bypassing security controls. Without code signing certificates users often have to go through multiple steps to execute an unsigned program. These certificates can be difficult to get, especially fraudulently (companies work hard to make this the case).
*   **Transport Layer Security (TLS) certificates** encrypt communication with websites, keeping web traffic private. Without these certificates most browsers will throw an error, warning the user their communication is insecure. Even worse is a mismatch between the certificate and the domain, which will throw a more severe error. These certificates used to be very difficult to get but thanks to services like LetsEncrypt they’ve become far easier recently.

Adversaries want both code signing and TLS certificates for modern attacks to avoid these browser and operating system level protections. Some adversaries instead try to encourage users to bypass security controls, but this can be risky both because users are more likely to not bother and there’s a higher chance the user might realize something seems off (and hopefully report it to their intrepid security team).

### Servers

Like most internet-based activities most adversaries rely on servers at one or many stages of their attacks. This includes delivery, either to host a downloaded file (for a link in a phishing email) or to host a browser exploit. In later stages servers can enable communication for command and control or as a target to upload stolen files or credentials. These may be adversary owned and run (bare metal) but, like so many organizations many adversaries have moved to using cloud servers or even directly using public web services. Both of these have the advantage of blending in.
