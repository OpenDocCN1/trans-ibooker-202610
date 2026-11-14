
## Key Analysis Questions

This is often the first question that executives ask, but it is not the first question that we mention for a specific reason. Whatever it is that makes you a valid target for a specific criminal group may not be unique to that group; the same information may be targeted by another group with similar goals. Therefore, it’s good not to focus exclusively on the group that you happened to catch this particular instance and lose sight of the overarching threats. Once you do understand their goals, however, it can be useful to understand more about the particular attackers. Analyzing information about attackers can include things such as the tactics they employ, what they target, how careful and cautious they were, what hours they operate, what infrastructure they use, whether it appears to be an individual or a group, and any other patterns that can be identified by analyzing the data.

### How could this have been prevented?

A significant goal of analysis is to understand what happened and why it happened so that it can be prevented in the future. When answering this question, you will focus on the things that went wrong within your own network. Were there unpatched vulnerabilities that the attacker exploited? Were there IDS alerts that were triggered but that no one looked into? Did a user reuse a password that had previously been leaked as part of an unrelated intrusion? This is not usually a fun analysis to conduct, because no one likes to hear or see what they did wrong, but if your organization simply wipes a piece of malware from a system without understanding or addressing how it got there, then you will likely have to go through the whole incident-response process again, as the root cause was not identified or addressed.

### How can this be detected?

This is where all of the delightful indicators you collected come into play. After the sometimes painful process of analyzing how you could have prevented the attack, it is nice to know that you can put things into place to prevent or detect future attacks. What you are able to do will depend greatly on the security systems that you have in place. While answering this question, it is important to focus on aspects that are unique to this particular attack, such as malware hashes and command-and-control IP addresses, as well as the aspects of the intrusion that are not as ephemeral, such as the systems that were targeted or the tactics utilized as the attackers moved through the network.

### Are there any patterns or trends that can be identified?

This type of analysis is especially relevant when comparing internal incidents to incidents that have been reported either through information-sharing groups or through open-source channels. When answering this question, you can attempt to identify patterns at various levels—from patterns related to targeting of organizations that may indicate a campaign, to patterns that identify reused or shared attack infrastructure, or patterns in social engineering avenues used by an attacker.

The output of the analysis that you conduct in this phase should enable action, whether that action is updating a threat profile, patching systems, or creating rules for detection. Focusing on the preceding questions and any other questions or requirements specific to your organization, will help ensure that the work that you do in this phase will be able to cycle back into the operational phases of `F3EAD`.

## Enriching Your Data

Once you have identified the questions you are trying to answer through analysis, you may need to move onto the next step of the mini-intelligence cycle - identifying any additional collection that is needed. Fortunately, we have spent a great deal of time on collection throughout the intelligence-drive incident response process and we even exploited the data - aka organized, sorted, and made sure we could fully leverage it in analysis. At this point however, you may realize that to answer the specific question you are focusing on you need additional information about some of the data that has already been collected. This process is called enrichment, and is useful whether you are trying to answer tactically-focused questions around a particular malware family or are answering more strategic questions around why you were targeted.

`Enrichment data` contains additional details about an indicator or a pattern of activity that usually aren’t used for detection, but for understanding more about a particular indicator and what it might mean if it is seen. This category of enrichment data can include things such as `WHOIS` information, autonomous system number (ASN), website content, recent and historical domain resolutions, associated malware, and many other additional details. The point of enrichment data is to gather more context around an indicator you have already identified so that you can better interpret its meaning. When looking at enrichment data in the analysis phase, you should focus on the patterns that emerge from the data rather than getting too caught up on one specific piece of information. One of the main reasons that many people end up with false positives and hundreds of thousands of indicators in block lists is that they take enrichment information and treat it as an indicator.

### Enrichment sources

The types of enrichment data that will be used depends on the indicator you are investigating and your goals for the analysis. Most enrichment sources provide information that is beneficial for multiple use-cases, but some are specific, so make sure that you know what you are looking for before spending a significant amount of time digging into a particular enrichment source. With all enrichment sources, it is key to record the date that the data was identified, as it is likely to change in the future. There is nothing more frustrating than finding a piece of information that is key to your analysis, and not being able to identify when or how you found it after it changes again!

Here are some of the types and sources of enrichment information:

#### WHOIS information

One of the most basic ways to get additional context and information about a domain or an IP address used in an attack is to get information about who registered or owns it. The `WHOIS` protocol, defined in [RFC 3912](https://tools.ietf.org/html/rfc3912), was originally intended to pass additional information about the users of the original internet, ARPANET. You could originally get `WHOIS` information through a command-line query, and that functionality still works today, though additional resources are available now, including websites and tools to capture current and historical data.

`WHOIS` has gone through several updates, as the user base grew and the scope of the internet expanded greatly. Currently, `WHOIS` contains information on the registrant’s name, email address, and additional contact information. `WHOIS` information can enrich analysis in several ways:

*   Tracking attacker infrastructure
    *   Some (not all!) attackers reuse information when registering domains. By identifying a name or pseudonym used by a malicious actor, it may be possible to identify additional malicious domains related to the same attacker group.
*   Identifying compromised domains
    *   In many cases, a legitimate domain may have been compromised and is being used by an attacker. Knowing the `WHOIS` information can help identify whether a domain is being run by an attacker or has just been compromised.
*   Identifying researcher-run infrastructure and sinkholes
    *   Many researchers on the internet carry out activity that looks similar to attackers, but is used for research or identifying vulnerabilities before actual attackers do. In most cases, the IP addresses used for this research will be identified through the `WHOIS` record, which can prevent an analyst from spending too much time digging into a nonmalicious IP address.

#### Passive DNS information
