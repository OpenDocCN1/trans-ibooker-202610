
# Command and Control (C2) Communication

Once an adversary has established persistence, especially if they've chosen the RAT route, they need a method to deliver commands (either via push or pull). Command and Control (C2) communication are carried out using a variety of methods and can leverage different ports, protocols, and services. Adversaries have been known to be incredibly creative in setting up C2 processes. One notable example is Turla, a group known to get innovative with command and control channels. In one instance they used an implant that communicated via encoded commands on popstar Britany Spears' Instagram comments. In the past, many pieces of malware, especially distributed denial-of-service (DDoS) tools, communicated by joining IRC channels or HTTP calls to a server under the adversary's control. The [Comment Crew](http://symc.ly/2tHeLKX) got its moniker from doing command-and-control with HTML comments on otherwise innocuous-looking web pages. Some adversaries use multiple methods, including DNS lookups, social media, or popular cloud applications.

Adversaries focus on making sure that their communication channels avoid notice and provide enough bandwidth to meet the needs of the adversary. In some cases, malware may communicate using only a few lines of text a day, while others include full virtual desktop capability.

## Actions on Objective

In most cases, all of this is not the ultimate goal, but rather the setup. Adversaries go through the process of setting up access in order to give themselves the capability to affect the target in a way they didn't have before. We call this new capability the *actions on objective (AoO)*.

While many of the earliest AoO were as simple as exploration, since the mid 1990s the most common action on objective has been collecting and exfiltrating information on target systems. In a very simple way, it makes a lot of sense: computers are ultimately about processing information. Supposedly infamous bank robber Willie Sutton was once asked why he robbed banks. Sutton responded "Because that's where the money is." In the modern age where money is data (as ones and zeros in a ledge or blockchain), plans for the next military technology are in computer aided design and development files on corporate servers, and political intelligence is stored as PDFs or Excel spreadsheets is it any wonder data theft became the dominant action on objective?

Next to data exfiltration the most common action on objective, and goes along with the network persistence piece of installation, is pivoting. In many cases the true target an adversary is after isn't available to attack directly. People shouldn't be opening phishing emails from the FTP server that holds their most valuable plans (they often do unfortunately) and shouldn't be browsing shady websites from the system with all their customer data (they do that too) but in the cases where these practices are followed the adversary will often attack related systems that may facilitate further access to get to the ultimate target. In many cases the adversary wouldn't even know what system they ultimately targeted, but would attack anything available, establish persistence, and pivot; essentially starting the entire intrusion process over again (kill chains within kill chains). A common intrusion pattern has been for the initial system to be quite innocuous and unrelated to anything (such as the adversary targeting a recruiter or accountant, who's likely to have Microsoft Office macros enabled), get admin credentials by cracking the Security Account Manager (SAM) file, pivot to the Domain Controller, steal the SAM file once again. Now the adversary has network wide administrator credentials, and can go after their ultimate target directly.

Aside from information theft and pivoting the most common actions on target by attackers, not just on computers, were categorized by the US Air Force as follows (We'll talk more about these a bit later on in regards to what defenders can do to respond to adversaries):

### Destroy

The adversary destroys a physical or virtual item. This could mean destroying data, overwriting or deleting files, or otherwise making a system unavailable until it is completely rebuilt. This could also mean destroying a physical object, though this is a rare occurrence in computer attacks, for example the Stuxnet destruction of Iranian centrifuges.^([1](#ch03.html#idm46530557019536))

### Deny

The adversary denies usage of a resource (such as a piece of infrastructure or a given capability) by the target, such as in the case of denial-of-service attacks that do not permit access to a site. Another example that has gained attention in recent years is ransomware, which encrypts a user's data and requires payment before the adversary will (in theory) decrypt the data for use again.

### Degrade

The adversary degrades the utility of the target's pieces of infrastructure or capabilities. This most often refers to the target's ability to control and command infrastructure, such as disabling networked services. A slightly less nuanced version of this, with a simple killchain, is distributed denial of service.

### Disrupt

By interrupting the flow of information, an adversary can disrupt the target's ability to carry out normal operations.

### Deceive

The adversary seeks to cause the target to believe something that is not true. In this context, the adversary may be inserting false information into a workflow to redirect assets or information, or cause the target to take a course of action to the adversary's benefit.

Most of these are straightforward and basic on the surface. However, the manner in which an adversary puts these together is key and often speaks directly to the adversary's identity and goals. An adversary can often hide malware, obfuscate command and control, and so forth; but ultimately the actions on the objective cannot be obfuscated, encoded, or protected. To steal information, an adversary must steal the files. To execute a DoS, an adversary must use compromised hosts to send massive amounts of network traffic. In short, the Actions on Objective phase can't be faked.

It's also important to understand that an adversary may combine multiple actions and vectors, including physical/non-cyber actions. This could include anything from recruiting an insider to leak strategic information to a *kinetic action* (for example, bombing a geographic site).

As you can see in [Figure 3-3](#ch03.html#fig_3tk), the adversary's kill chain stays tightly defined while the incident cycle has to react to it, which starts in the Identification phase. Identification can occur anywhere between the Identification and the Actions on Target phases of the kill chain, and causes dramatically different incident responses to occur. An incident identified during the Delivery phase is ideal. The defenders can block the attack at an email or web proxy and keep the attack from ever executing. An attack detected during a later phase, such as Command and Control or Actions on Objective, is likely going to be painful, involving many compromised resources and an expensive and lengthy incident-response investigation.

![](assets/image-to-come.png)

## Example Kill Chain

To illustrate the kill chain, imagine a series of attacks by a fictitious attack group codenamed Grey Spike. This group conducts political intelligence collection against multiple countries. Most recently, reporting indicated that they have been targeting election campaigns in the US and UK. They seek to gain information about candidates' stances on economic, foreign policy, and military issues. Here's what their strategy would entail:

### Targeting

Grey Spike does not choose its own targets but receives tasking (intelligence jargon for what data and organizations they're instructed to target) from national policy makers. This tasking describes specific target countries and candidates as well as key words of interest.

### Reconnaissance
