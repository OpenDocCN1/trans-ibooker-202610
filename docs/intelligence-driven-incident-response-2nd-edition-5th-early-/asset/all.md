![](assets/cover_ER.png)

![](assets/cover_ER.png)

# Intelligence-Driven Incident Response

by Scott  J.  Roberts and Rebekah  Brown

Copyright © 2023 Rebekah Brown and Scott Roberts. All rights reserved.

Printed in the United States of America.

Published by O’Reilly Media, Inc., 1005 Gravenstein Highway North, Sebastopol, CA 95472.

O’Reilly books may be purchased for educational, business, or sales promotional use. Online editions are also available for most titles ([http://oreilly.com/safari](http://oreilly.com/safari)). For more information, contact our corporate/institutional sales department: 800-998-9938 or *corporate@oreilly.com*.

*   Acquisitions Editor: Jennifer Pollock
*   Development Editor: Angela Rufino
*   Production Editor: Elizabeth Faerm
*   Copyeditor: TO COME
*   Proofreader: TO COME
*   Indexer: TO COME
*   Interior Designer: David Futato
*   Cover Designer: Karen Montgomery
*   Illustrator: Kate Dullea

*   October 2023: Second Edition

Add additional revdate spans below as needed. 

# Revision History for the Early Release

*   2022-02-25: First Release
*   2022-08-18: Second Release
*   2022-11-18: Third Release
*   2023-02-07: Fourth Release
*   2023-04-07: Fifth Release

See [http://oreilly.com/catalog/errata.csp?isbn=9781098120689](http://oreilly.com/catalog/errata.csp?isbn=9781098120689) for release details.

The O’Reilly logo is a registered trademark of O’Reilly Media, Inc. *Intelligence-Driven Incident Response*, the cover image, and related trade dress are trademarks of O’Reilly Media, Inc.

The views expressed in this work are those of the authors, and do not represent the publisher’s views. While the publisher and the authors have used good faith efforts to ensure that the information and instructions contained in this work are accurate, the publisher and the authors disclaim all responsibility for errors or omissions, including without limitation responsibility for damages resulting from the use of or reliance on this work. Use of the information and instructions contained in this work is at your own risk. If any code samples or other technology this work contains or describes is subject to open source licenses or the intellectual property rights of others, it is your responsibility to ensure that your use thereof complies with such licenses and/or rights. PROD: Uncomment the following sentence if appropriate and add it to the above para: This book is not intended as \[legal/medical/financial; use the appropriate reference\] advice. Please consult a qualified professional if you require \[legal/medical/financial\] advice.

978-1-098-12062-7

\[LSI\]

# Chapter 1. Introduction

> But I think the real tension lies in the relationship between what you might call the pursuer and his quarry, whether it’s the writer or the spy.
> 
> John le Carre

Once relegated to the secretive realms of national security and military operations, intelligence has become something that is fundamental to the daily functioning of many organizations around the world. At its core, intelligence seeks to give decision makers the information that they need to make the right choice in any given situation.

Previously, decision makers experienced significant uncertainty because they did not have enough information to make the right decisions. Today they are just as likely to feel like there is too much information, but just as much ambiguity and uncertainty. This is especially the case with network security, where there are fewer traditional indications that a significant action is actually about to take place. In order to make decisions about how to prepare for and respond to a network security incident, decision makers need analysts who understand intelligence fundamentals, the nuance of network intrusions, and how to combine the two into an accurate assessment of a situation and what it means for their entire organization. In short, they need analysts who can conduct intelligence-driven incident response.

Before diving into the application of intelligence-driven incident response, it is important to understand the evolution of cyber security incidents and their responses, and why it is so relevant in this field. This chapter covers the basics of cyber threat intelligence, including its history, recent activity, and the way forward, and sets the stage for the concepts discussed in the rest of this book.

# Intelligence as Part of Incident Response

As long as there has been conflict, there have been those who watched, analyzed, and reported observations about the enemy. Wars have been won and lost based on an ability to understand the way the enemy thinks and operates, to comprehend their motivations and identify their tactics, and to make decisions—large and small—based on this understanding. Regardless of the type of conflict, whether a war between nations or a stealthy intrusion against a sensitive network, intelligence guides both sides. The side that masters the art and science of intelligence, analyzing information about the intent, capability, and opportunities of adversaries, and is able to act on that information, will almost always be the side that wins.

## History of Cyber Threat Intelligence

One of the best ways to understand the role of intelligence in incident response is by studying the history of the field. Each of the events listed below could (and often do!) fill entire books. From the iconic book *The Cuckoo’s Egg* to recent revelations in decades old intrusions such as Moonlight Maze, the history of cyber threat intelligence is intriguing and engaging, and offers many lessons for those working in the field today.

### The First  Intrusion

In 1986, Cliff Stoll was a PhD student managing the computer lab at Lawrence Berkeley National Laboratory in California when he noticed a billing discrepancy in the amount of 75 cents, indicating that someone was using the laboratory’s computer systems without paying for it. Our modern-day network security-focused brains see this and scream, “Unauthorized access!” but in 1986 few administrators would have jumped to that conclusion. Network intrusions were not something that made the news daily, with claims of millions or even billions of dollars stolen; most computers connected to the “internet” belonged to government and research institutes, not casual users, and it was easy to assume everyone using the system was friendly. The network defense staple tool tcpdump was a year from being started. Common network discovery tools such as Nmap would not be created for another decade, and exploitation frameworks such as Metasploit would not appear for another 15 years. The discrepancy was more easily expected to be a software bug or bookkeeping error as it was that someone had simply not paid for their time.

Except that it wasn’t. As Stoll would discover, he was not dealing with a computer glitch or a cheap mooch of a user. He was stalking a “wily hacker” who was using Berkeley’s network as a jumping-off point to gain access to sensitive government computers, such as the White Sands Missile Range and the National Security Agency (NSA). Stoll monitored incoming network traffic with printers writing reams of packets onto paper to keep a record and began to profile the intruder responsible for the first documented case of cyber espionage. He learned the typical hours the attacker was active, monitored the commands he ran to move through the interconnected networks, and observed other patterns of activity. He discovered how the attacker was able to gain access to Berkeley’s network in the first place by exploiting a vulnerability in the *movemail* function in GNU Emacs, a tactic that Stoll likened to a cuckoo bird leaving its egg in another bird’s nest to hatch and inspiring the name of his book on the intrusion, *The Cuckoo’s Egg*.

Understanding the attacker meant that it was possible to protect the network from further exploitation, identify where he may target next, and allowed a response, both on the micro level (identifying the individual carrying out the attacks) and on the macro level (realizing that nations were employing new tactics in their traditional intelligence-gathering arsenal and changing policies to respond to this change). Sharing this understanding ended up being key not just to protecting Lawrence Berkeley National Lab, but many other government organizations.

### Destructive  Attacks

In 1988 Cornell student Robert T. Morris hacked into a computer lab at the Massachusetts Institute of Technology (MIT) and released a computer program that was designed to replicate itself to as many computers as possible without being detected. It did this by exploiting a backdoor in the Internet’s email delivery system as well as a flaw in the *finger* program that identified network users. It may have just been a harmless experiment - or a prank, as some have described it - that few people ever knew about, however it did not work exactly as Morris intended and became part of cyber history (which is just like regular history, but cooler). The worm ended up crashing 6000 computers, which was about 10% of the internet of the time, with computers at Harvard, Princeton, Stanford, Johns Hopkins, NASA, and the Lawrence Livermore National Laboratory among the many victims. Investigators from the FBI were struggling to inspect the activity, unsure if it was an outage or an attack, when Morris called two friends and admitted that he was the one behind the worm. One of the friends called *The New York Times,* which led to the eventual identification and conviction of Morris for violations of the new Computer Fraud and Abuse Act of 1986 (CFAA).

While there was no nefarious intent behind this worm- it was really just another example of how programs don’t always behave exactly the way that their creator intended them to - there were far reaching implications that can still be seen today. As the internet became more and more critical to operations, it became even more important to be able to quickly identify and remediate other intrusions or destructive attacks. The Computer Emergency Response Team (CERT) was established at Carnegie Mellon University as a professional, trained response team responsible for providing assessments and solutions for cyber attacks. It also highlights why it is sometimes important to be able to attribute an attack - in 1988 there had been significant “warming” of the Cold War, with Reagan and Gorbachev meeting in Moscow and Soviet Troops beginning to withdraw from Afghanistan. If this worm had inaccurately been blamed on Soviet activity - which was actually the case for the activity from the Cuckoo’s Egg - it could have significantly changed the course of history.

### Moonlight  Maze

In the decade following 1986’s Cuckoo’s Egg and 1988’s Morris Worm, the field of Incident Response improved, not just with the creation of CERT, but because of the professionalization of the field itself across the government, military, and private sector. In addition, the emergence of proper network monitoring tools meant that defenders weren’t reliant on printers scattered around a basement to identify malicious network activity. This increase in capabilities was fortuitous, as intrusions not only continued but grew in scope and sophistication. In 1998, the US government identified what is believed to still be the largest and longest running intrusion into government networks - codenamed Moonlight Maze.

In March of 1998, the US government noticed anomalous activity within several sensitive and restricted networks, including the Pentagon, NASA (yes, NASA has apparently always been pretty high on the priority list for intrusions), and the Department of Energy (DOE). Further analysis identified the same malicious activity at several universities and identified that the activity had been ongoing for at least two years. The sustained nature of this activity was unlike any of the previous intrusions (as far as was known at the time) which had seemed more targeted and short-lived. Unlike those instances, the adversaries had left strategic backdoors in different parts of the network so that they could return at will. Information was being gathered from numerous locations that often seemed unrelated.

We have been fortunate enough to work closely with people who directly supported the investigation and response to Moonlight Maze, both in the 1990s and today, as there are still many unknowns and many insights to be found from this intrusion. Talking with these individuals and reading through the numerous reports on the intrusion hammers home the point that intelligence work is critical to incident response, and that cyber threat intelligence in particular bridges the gap between what is happening on the strategic level with national interests and foreign adversaries, and how those adversarial goals and actions show up on a computer network. The US has had a full scope intelligence community actively looking and listening for signs of foreign interference, but the largest network attack went unnoticed until it was detected “by accident” because intelligence work had not yet been fully modified to account for actions taken against a network.

Moonlight Maze kicked cyber threat intelligence capabilities into the modern era. Computer networks were not something that might be impacted from time to time, these networks were now being targeted directly for the information and access they held. Computer networks were part of intelligence collection, and intelligence needed to play a role in their defense.

## Modern Cyber-Threat Intelligence

Over the decades, the threats have grown and morphed. Adversaries are not just foreign governments or curious students. Organized criminals, identity thieves, scammers, ideologically motivated activists, and others with various motivations all realized the impact that their activities could have when directed at digital targets instead of physical ones. These adversaries use an ever-expanding set of tools and tactics to attack their victims and actively attempt to evade detection. At the same time our reliance on our networks has increased, making incidents even more impactful. Understanding the attacker has gotten much more complicated, but no less important.

Understanding how to identify the attacker activity and how to use that information to protect networks is the fundamental concept behind a more recent addition to the incident responder’s toolkit: cyber-threat intelligence. *Threat intelligence* is the analysis of adversaries—their capabilities, motivations, and goals, and cyber-*threat intelligence* (CTI) is the analysis of how adversaries use the cyber domain to accomplish their goals. See [Figure 1-1](#ch01.html#figure_1_1_from_intelligence_to_cyber_threat_intelligence) on how these levels of attacks play into one another.

![](assets/image-to-come.png)

Initially, intelligence analysts came into the picture after an intrusion like Moonlight Maze to understand what the overall intrusion told us about the adversary. What were their goals, their motivations, their capabilities, their organizational structure - all things that were important for a strategic understanding and long-term planning, but not things that would immediately provide value to those trying to defend their networks from the attacks of today, tomorrow, or sometimes even last week. Cyber-threat intelligence began to focus more on tactical and technical details that were more immediately actionable, learning along the way what types of information were most valuable in different situations. Cyber-threat intelligence analysts didn’t just bring data, they also brought insights.

In information security, we traditionally focus on observable concepts; we like things that are testable and reproducible. Cyber threat intelligence, meanwhile, lives in the area between observations and interpretation. We may not know ***for sure*** that an adversary will attempt to access employee financial records, but we can conduct analysis on data about past intrusions and successful attackers outside of our network and make recommendations on the systems and types of data that may need additional protection. Not only do we need to be able to interpret information, but we also need to be able to convey it in a way that is meaningful to the intended audience to help them make decisions. Looking back on his historic analysis of the intrusions in the Cuckoo’s Egg, Cliff Stoll identified the need for a story as one of his key takeaways from the entire experience. “I thought I could just show people the data and they would understand,” he said “but I was wrong. You need to tell a story.” (SANS CTI Summit 2017)

## The Way  Forward

New technologies give us more information about the actions that attackers take as well as additional ways to act on that information. However, we have found that with each new technology or concept implemented, the adversary adapted; worms and viruses with an alphabet soup of names changed faster than our appliances could identify them, and sophisticated, well-funded attackers were often more organized and motivated than many network defenders. Ad-hoc and intuitive intelligence work would no longer suffice to keep defenders ahead of the threat. Analysis would need to evolve as well and become formal and structured. The scope would have to expand, and the goals would have to become more ambitious.

In, addition to detecting threats against an organization’s often nebulous and ephemeral perimeter, analysts would need to look deeper within their networks for the attacks that got through the lines, down to individual user systems and servers themselves, as well as look outward into third-party services and to better understand the attackers who may be targeting them. The information would need to be analyzed and its implications understood, and then action would have to be taken to better prevent, detect, and eradicate threats. The actions taken to better understand adversaries would need to become a formal process and a critical part of information security operations: threat intelligence.

# Incident  Response  as a Part of Intelligence

*Intelligence* is often defined as data that has been refined and analyzed to enable stakeholders to make better decisions. Intelligence, therefore, requires data. In intelligence-driven incident response, there are multiple ways to gather information that can be analyzed and used to support incident response. However, it is important to note that incident response will also generate cyber-threat intelligence. The traditional intelligence cycle—which we cover in depth in [#basics\_of\_intelligence](https://atlas.oreilly.com/oreillymedia2/intelligence-driven-incident-response/editor/master/01-introduction.html#basics_of_intelligence)—involves direction, collection, processing, analysis, dissemination, and feedback. Intelligence-driven incident response involves all of these components and helps inform direction, collection, and analysis in other applications of threat intelligence as well, such as network defense, secure software development, and user awareness training. Intelligence-driven incident response doesn’t end when the intrusion is understood and remediated; it generates information that will continue to feed the intelligence cycle.

Analysis of an intrusion, no matter if it was successful or failed, can provide a variety of information that can be used to better understand the overall threat to an environment. The root cause of the intrusion and the initial access vector can be analyzed to inform an organization of weaknesses in network defenses or of policies that attackers may be abusing. The malware that is identified on a system can help identify the tactics that attackers are using to evade traditional security measures such as antivirus or host-based intrusion-detection tools and the capabilities they have available to them. The way an attacker moves laterally through a network can be analyzed and used to create new ways to monitor for attacker activity in the network. The final actions that an attacker performed (such as stealing information or changing how systems function), whether they were successful or not, can help analysts understand the enemy’s motivations and goals, which can be used to guide overall security efforts. There is essentially no part of an incident-response engagement that cannot be used to better understand the threats facing an organization and improve future defense and response.

For this reason, the various processes and cycles outlined in this book are aimed at ensuring that intelligence-driven incident response supports overall intelligence operations. Although they provide specific guidance for utilizing cyber-threat intelligence in incident response, keep in mind that wider applications can be used as intelligence capabilities expand.

# What Is  Intelligence \-Driven Incident Response?

Cyber-threat intelligence isn’t a new concept, simply a new name for an old approach: applying a structured analytical process to understand an attack and the adversary behind it. The application of threat intelligence to network security is more recent, but the basics haven’t changed. Cyber-threat intelligence involves applying intelligence processes and concepts—some of the oldest concepts that exist—and making them a part of the overall information security process. Threat intelligence has many applications, but one of the fundamental ways it can be utilized is as an integral part of the intrusion-detection and incident-response process. We call this *intelligence-driven incident response* and think it is something every security team can do, with or without a major capital investment. It’s less about tools, although they certainly help sometimes, and more about a shift in the way we approach the incident-response process. Intelligence-driven incident response will help not only to identify, understand, and eradicate threats within a network, but also to strengthen the entire information security process to improve those responses in the future.

# Why  Intelligence \-Driven Incident Response?

Over the past few decades, our world has become increasingly interconnected, both literally and figuratively, allowing attackers to carry out complex campaigns and intrusions against multiple organizations with the same effort that it used to take to target a single entity. We are long past the point where we can automatically assume that an intrusion is an isolated incident - in fact, while we used to be stunned to find overlaps and connections between intrusions, now we are suspicious when we *don’t* see overlap. When we better understand the adversary, we can more easily pick up on the patterns that show commonalities between intrusions. Intelligence-driven incident response ensures that we are gathering, analyzing, and sharing intelligence in a way that will help us identify and respond to these patterns more quickly.

## Operation  SMN

A good example of this is the analysis of the Axiom Group, which was identified and released as a part of a Coordinated Malware Eradication (CME) campaign in 2014 called [Operation SMN.](http://www.novetta.com/wp-content/uploads/2014/11/Executive_Summary-Final_1.pdf)

For more than six years, a group of attackers known as the Axiom Group stealthily targeted, infiltrated, and stole information from Fortune 500 companies, journalists, nongovernmental organizations, and a variety of other organizations. The group used sophisticated tools, and the attackers went to great lengths to maintain and expand access within the victims’ networks. As malware was detected and the incident-response process began within various victim organizations, coordinated research on one of the malware families used by this group identified that the issue was far more complex than originally thought. As more industry partners became involved and exchanged information, patterns began to emerge that showed not just malware behavior, but the behaviors of a threat actor group working with clear guidance. Strategic intelligence was identified, including regions and industries targeted.

This was an excellent example of the intelligence cycle at work in an incident-response scenario. Not only was information collected, processed, and analyzed, but it was disseminated in such a way as to generate new requirements and feedback, starting the process over again until the analysts had reached a solid conclusion and could act with decisiveness, eradicating 43,000 malware installations at the time that the report was published. The published report, also part of the dissemination phase, allowed incident responders to better understand the tactics and motivations of this actor group.

## SolarWinds

In December of 2020, news broke of a massive intrusion at the Texas-based company SolarWinds, which makes software for monitoring and managing IT networks and is a very popular tool in many large networks, including cybersecurity companies, governments, and Fortune 500 companies. The activity was detected after the cybersecurity firm FireEye, a SolarWinds customer, identified that their networks had been compromised and a set of tools they developed for identifying intrusions had been accessed by an unknown entity. Their own investigation into the intrusion on their network led them to identify SolarWinds as the source of the intrusion.

Their analysis indicated that the SolarWinds’ networks had been compromised in late 2019 and their software tampered with, so that a software update that was pushed to all its clients contained a backdoor that would allow the adversaries access to those networks as well. This was not the first software-based supply chain attack; however it was notable for its size and scale - estimates suggested that over 18,000 of SolarWinds’ customers were impacted. It was also notable for its response, which, 20 years after Moonlight Maze, showed how far cyber threat intelligence has come as a discipline. Once identified, FireEye published a blog post with details about the incident and ways for others to detect activity on their network. Additional teams jumped in to analyze activity on their networks and continued to share indicators and findings, both publicly and through established threat sharing groups, allowing a picture of the overall attack to quickly develop. The Department of Homeland Security published guidance on supply chain attacks, taking the response from a mentality of just reacting to one isolated incident to thinking about how this new information shapes the way the industry should think about preparing for intrusions in the future. While certainly not a perfect process, SolarWinds illustrates the direction that cyber threat intelligence can play in incident response and how it can help not only the organizations directly impacted but help to surface important lessons for others.

Both the Axiom Group attacks and the SolarWinds software supply chain intrusion were information-seeking, espionage-related attacks, but nation-state sponsored attackers aren’t the only thing that incident responders have to worry about. Financially motivated criminal activity is also evolving, and those actors are also working hard to stay ahead of defenders and incident responders. One of the most significant tactic changes for financially motivated criminals in recent years is the move to ransomware. Ransomware attacks use tools to encrypt data on a network and then charge a ransom for the key to decrypt the data. The concept of ransomware has been around for decades; however its usage has increased drastically since 2012, along with its impact. Although ransomware attacks do not always involve strategic and coordinated attacks against multiple organizations, the groups executing ransomware attacks often target different victims using the same tactics, toolsets, and often even targeting information. Defenders working against these financially motivated attacks can also leverage intelligence-drive incident response to identify early indications that their networks have been breached by these actors before the actual encryption process has begun.

# Conclusion

Despite the many advances in the computer security field, attackers continue to adapt - but they do not have to outpace defenders. Intelligence-driven incident response allows us to learn from attackers; to identify their motivations, processes, and behaviors; to identify their activities even as they seek to outwit our defenses and detection methods. The more we know about attackers, the better we can prevent, detect, and respond to their actions.

We have reached the point where a structured and repeatable process for implementing intelligence in the incident-response process is necessary, and this book aims to provide insight into that process. Throughout this book, we provide various models and methods that can be viewed as the building blocks of intelligence-driven incident response, as well as the background as to why these models are beneficial to and integrate with incident response. There is no one-size-fits-all approach. In many cases, the incident or the organization will dictate which specific combination of models and approaches fits best. Understanding the foundational principles of intelligence and incident response, as well as the specific methods for integrating them, will allow you to build a process for intelligence-driven incident response that will work for you and to develop that process to meet the needs of your organization.

## Sources :

[https://www.fbi.gov/news/stories/morris-worm-30-years-since-first-major-attack-on-internet-110218](https://www.fbi.gov/news/stories/morris-worm-30-years-since-first-major-attack-on-internet-110218)

https://www.sciencedirect.com/topics/computer-science/moonlight-maze

[https://www.mandiant.com/resources/evasive-attacker-leverages-solarwinds-supply-chain-compromises-with-sunburst-backdoor](https://www.mandiant.com/resources/evasive-attacker-leverages-solarwinds-supply-chain-compromises-with-sunburst-backdoor)

[https://threatpost.com/solarwinds-attackers-new-tactics-malware/176818/](https://threatpost.com/solarwinds-attackers-new-tactics-malware/176818/)

Marcus Willett (2021) Lessons of the SolarWinds Hack, Survival, 63:2, 7-26, DOI: [10.1080/00396338.2021.1906001](https://doi-org.ezproxy2.apus.edu/10.1080/00396338.2021.1906001)

http://www.novetta.com/wp-content/uploads/2014/11/Executive\_Summary-Final\_1.pdf

# Chapter 2. Basics of Intelligence

> It consists of gathering facts. … It consists of forming hypotheses on the basis of these facts, of testing these hypotheses for traces of one’s own ignorance or bias, of cleansing them if possible. The goal is to build better hypotheses than already exist and to establish them as relatively more true: it is to reveal a sharper picture of what happened and to make a closer approach to actuality than anyone has yet contrived.
> 
> Sherman Kent

Intelligence analysis is one of the oldest and most consistent concepts in human history. Every morning people turn on the news or scroll through feeds on their phones, looking for information that will help them plan their day. What is the weather report? What implications does that have for their activities for that day? How is the traffic? Do they need to plan for extra time to get to where they need to go? External information is compared to an internal set of experiences and priorities, and an assessment is made of the impact on the target subject—the individual in question.

This is the basic premise of intelligence: taking in external information from a variety of sources and analyzing it against existing requirements in order to provide an assessment that will affect decision making. This occurs at the individual level as well as at higher levels; this same process is implemented at the group, organization, and government level every single day. There is one big catch though - unlike many forms of day-to-day analysis, intelligence analysis involves trying to understand something about an adversary who very much wants to stay hidden from you. The weather report - although occasionally inaccurate - was not intentionally tricking you into leaving your umbrella at home so that you would get soaked in a downpour. Because of this, intelligence analysis almost always involves some aspect of secrecy. Even when it is not part of a classified government program, it involves an entity that does not want you to have the whole picture. Likewise, you don’t want that entity to know what you know about it, otherwise it might change tactics and cause you to start over at the beginning. In fact, a significant debate in intelligence work is around the concept of intelligence-gain-loss (which we will cover in depth in chapter $) where analysts have to determine how much intelligence value will be lost by taking an action that would warn the adversary that their presence, tactics, or tools have been identified.

Although there are a growing number of intelligence training programs inside and outside of traditional government and military fields, many individuals currently conduct some form of intelligence analysis on their own without formal training, and many security teams work through similar processes as they conduct investigations without realizing that they are, in fact, engaged in intelligence analysis. While intuitive analysis can be beneficial to a security program, it is even more useful when basic structures, such as processes and models, are utilized to streamline intelligence work, account for biases, and make the analytic judgements defensible and repeatable.

When businesses and governments conduct intelligence operations, it is based on a formalized process and doctrine that have been captured over the years. In addition, there are formalized processes which have been specialized for intelligence operations in information security and incident response. This chapter walks through key concepts, some from intelligence—some from security, and a few that combine both. We’ll start with abstract concepts that are primarily pulled from intelligence doctrine and move toward the more concrete concepts that can be applied directly to your incident-response investigations.

# Intelligence and Research

Intelligence as a discipline follows the same basic principles of other types of applied research, however there are several significant differences; secrecy, timeliness, and lack of reproducibility. The first, as we mentioned briefly above, is the understanding that intelligence often deals with matters that the subject or the target is actively trying to keep hidden. Researchers cannot go to an archive or search online repositories and find \*all\* of the information that they need. They may be able to find a great deal of relevant information, but there will always be an unspoken understanding that key pieces of information are intentionally not included in public information.

The second difference is that timeliness is far more significant with intelligence analysis than in other forms of research. If the information is not analyzed and presented to decision makers ahead of when it is needed, then it is likely no longer relevant. The third difference is that reproducibility, or being able to replicate findings, is often not done because of the first two principles. Most analysts will not have access to the exact same information to provide external validation of an analytic judgment, and the timeliness required of intelligence products means that the peer-review process is a rarity in the field, and often only conducted in the aftermath of an intelligence failure.

# Data Versus Intelligence

Before tackling anything else, it’s important to clear up one of the most important distinctions of this discussion: the difference between information, data, and intelligence. All of these are significant terms in the security community and unfortunately are often used interchangeably, to the point that many practitioners have a difficult time articulating the difference between them.

“Joint Publication 2-0,” the US military’s primary joint intelligence doctrine, is one of the foundational intelligence documents used today. In its introduction, it states, “Information on its own may be of utility to the commander, but when related to other information about the operational environment and considered in the light of past experience, it gives rise to a new understanding of the information, which may be termed intelligence.”

Data is a piece of information, a fact, or a statistic. Data is something that describes something that is. In our previous example about the weather report, the temperature is a piece of data. It is a fact, something that has been measured using a proven and repeatable process. Knowing the temperature is important, but in order to be useful for decision-making, it must be analyzed in the context of what else is going on that day. In information security, an IP address or a domain are pieces of data. Without any additional analysis to provide context, they are simply facts. When various data points are gathered and analyzed to provide insight around a particular requirement, it becomes intelligence.

Intelligence is derived from a process of collecting, processing, and analyzing data. Once it has been analyzed, it must be disseminated in order to be useful. Not only does intelligence need to be disseminated, but it needs to reach its intended audience in a timely manner. Intelligence that does not get to the right audience is wasted intelligence. Wilhelm Agrell, a Swedish writer and historian who studied peace and conflict, once famously said, “Intelligence analysis combines the dynamics of journalism with the problem solving of science.”

The difference between data and true intelligence is analysis. Intelligence requires analysis that is based on a series of requirements and is aimed at answering questions relevant to decision makers. Without analysis, most of the data generated by the security industry remains simply data. That same data, however, once it has been properly analyzed in response to requirements, becomes intelligence, as it now contains the appropriate context needed to answer questions and support decision-making.

# Sources and Methods

Now that we have cleared up the distinction between information, data, and intelligence, the natural next question is, “Where should I get this data from so that I can analyze it and generate intelligence?”

Traditional intelligence sources are most often centered around the INTs, which describe where the data is collected from:

HUMINT
: 

Human-source intelligence is derived from humans, either through covert or clandestine methods or from overt collection such as from diplomats. Human-source intelligence is the oldest form of intelligence collection. There is serious debate about whether cyber-threat intelligence can be derived from HUMINT, however there is a growing body of evidence to suggest that HUMINT can be a crucial part of the story. One example is interviews or conversations with individuals who are involved with or have firsthand knowledge of intrusions, such as when researchers from Kaspersky Labs were able to connect with a systems administrator from a network compromised as part of Moonlight Maze. Not only did the sysadmin give inside information and perspective that only a person who experienced an intrusion can, he also provided access to a server from the intrusion that had been sitting under his desk - HUMINT can often lead to additional data collection. Another example that many describe as HUMINT is information gained from interactions with individuals via restricted or members-only online forums. This type of intelligence gathering could also be considered SIGINT, as it is derived from electronic communications.

SIGINT
: 

Signals intelligence includes intelligence derived from the interception of signals, including communications intelligence (COMINT), electronic intelligence (ELINT), and foreign instrumentation signals intelligence (FISINT). Most technical intelligence collection falls under SIGINT, because after all, computers function using electronic signals, so anything derived from a computer or other networking device could be considered SIGINT.

OSINT
: 

Open source intelligence is gathered from publicly available sources, including news, social media, and commercial databases as well as a variety of other nonclassified sources. We discussed previously that intelligence analysis, including cyber threat intelligence, involves some aspect of secrecy. This does not mean, however, that ALL intelligence sources must be secret. Published reports on cyber-security threats is one type of OSINT that can be incredibly useful in intelligence analysis. When dealing with government-backed actors, publications that detail the organizational structure of that government’s offensive cyber forces can provide a wealth of knowledge. OSINT can also help reveal technical details about things like IP addresses or domain names that are publicly accessible; for example, a WHOIS query detailing who registered a malicious domain.

IMINT
: 

Imagery intelligence is collected from visual representations, including photography and radar. IMINT is not typically a source of cyber-threat intelligence, however there are always cases where visual representations can provide critical information, such as the ability to watch troop movements during large scale, government-backed denial of service attacks, as were previously seen with the Russia and Georgia conflict.^([1](#ch02.html#idm46530565004560))

MASINT
: 

Measurement and signature intelligence is gathered from technical means, excluding signal and imagery. MASINT often includes signatures from nuclear, optical, radio frequency, acoustics, and seismic signatures. As MASINT specifically excludes signals intelligence, it is also not a typical source of cyber-threat intelligence.

GEOINT
: 

Geospatial intelligence is collected from geospatial data, including satellite and reconnaissance imagery, maps, GPS data, and other sources of data related to locations. Some organizations consider IMINT to be a part of GEOINT, and some believe it is a separate discipline. Similar to IMINT, GEOINT is not a typical source of cyber-threat intelligence, but it can provide contextual information on threats to help you understand how attackers may use the cyber domain to achieve their goals.

At this point, many would bring up a variety of other INTs that have popped up over the years, including cyber intelligence (CYBINT), technical intelligence (TECHINT), financial intelligence (FININT), and the most recent one we found - CyberHumint - but most of these new terms are already covered by other intelligence-collection methods. For example, cyber intelligence is primarily derived from ELINT and SIGINT. It is not important to get into an argument about the number of INTs in existence; the important thing is to understand the source of the data. At the end of the day, if it helps to refer to a specific collection type as its own INT, then go ahead; just be prepared to deal with the eventual terminology conflicts that tend to pop up in this field.

In addition to the traditional intelligence-collection disciplines listed here, some collection methods are often utilized specifically in cyber threat intelligence. It is useful to have a solid understanding of where this specific threat data comes from:

Incidents and investigations
: 

This data is collected from the investigation of data breaches and incident-response activities. This is often one of the richest data sets used in cyber-threat intelligence because investigators are able to identify multiple aspects of the threat, including the tools and techniques that are used, and often can identify the intent and motivation behind the intrusion.

Honey pots and honey nets
: 

These devices are set up to emulate machines or entire networks and gather information about interactions with these devices. There are many types of honey pots: low interaction, high interaction, internal honeypots, and honeypots on the public internet. Honeypot information can be useful as long as you know the type of honeypots it comes from, what they were monitoring for, and the nature of the interactions. Traffic gathered from a honeypot that captures exploit attempts or attempts to install malware on a system are far more useful in analysis than scanning or web-scrapping traffic.

Forums and websites
: 

A variety of companies claim to have deep web or dark web collections. In many cases, these companies are referring to forums and chatrooms with restricted access that are not easily accessible from the internet. In these forums and sites, individuals often exchange information that is valuable after it’s analyzed. There are so many of these types of sites that it is nearly impossible for any one company to have complete coverage of *the dark net*, so be aware that the collection is often limited in scope and will differ from that of other companies that claim to have similar data.

Even these techniques are new iterations of common techniques of the past. What’s old is new as technology evolves, and intelligence is no different. The philosopher George Santayana’s missive about forgetting the past is as true as ever.

# Models

Models are a critical tool in the analyst’s toolkit. Without models, many analysts would not be able to keep up with the quantity of data that exists in the world or the requirements that they provide meaning around this data. They would struggle to synthesize the information that they collected from all the various sources we just discussed and make meaning and insight that are relevant to decision makers.

One of the best working definitions of models for intelligence purposes comes from the book “Quantitative Intelligence Analysis” - “Models refer to the cognitive, conceptual, mathematical, physical, or otherwise logical representation of systems, entities, phenomena, or processes.” The two most common types of models used in cyber threat intelligence are mental models and conceptual models. Mental models are cognitive models that hold an analyst’s perception of reality. Even when analysts have gone through similar courses of formalized training, their mental models may be very different because of the many diverse ways that individuals perceive and process information.

Conceptual models on the other hand, are representations of explicit knowledge, and are commonly the result of intelligence synthesis based on mental models that have been codified. Some of the most useful models have been derived from an analyst capturing and codifying a way that they intuitively approached or thought about problems. One example of this is the Diamond Model for Intrusion Analysis, which we cover in depth in Chapter 3, The Basics of Incident Response. Sergio Caltegrione, one of the creators of the model, described the generation of a new model as “a long progression of understanding the work we were repeatedly doing, until we understood it well enough to abstract it. Once abstracted, it became infinitely more useful because we could then ask new questions \[of the model\].” Sergio also described models as formulas - you have to understand its parts and their relationships before you can fully utilize them.

The goal of codifying a conceptual model is to be able to structure information so that it can be analyzed and acted on. Many models that are frequently used in intelligence analysis, including the diamond model, are covered further in Chapters [3,](https://atlas.oreilly.com/oreillymedia2/intelligence-driven-incident-response/editor/master/02-basics-of-intelligence.html#basics_of_incident_response) the Basics of Incident Response, and 7, Analyze.

## Using Models for Collaboration

One of the key benefits of using explicit, conceptual models (rather than exclusively using mental models) is that it enables collaboration. Intelligence collaboration has been likened to “thinking in public”, because it requires analysts to verbalize or otherwise articulate mental processes of analysis and synthesis that can be difficult to describe. Models are critical to analytic collaboration and will result in higher quality intelligence, and therefore it is worth the time investment to make sure that team members understand the different conceptual models that are commonly used and also understand the process for codifying the mental models that they commonly use in analysis.

## Process Models

Models can be divided into two broad categories: models that represent our thinking, and models that represent the subject of analysis. The first type of model can be thought of as being used to give structure to processes - such as how we think, or the process of generating intelligence. This section covers two models that are used to effectively generate and act on intelligence. The first is the OODA loop, which can be used in making quick, time-sensitive decisions. The second is the intelligence cycle, or intelligence process, which can be used to generate more-formal intelligence products that will be used in a variety of ways, from informing policy to setting future intelligence requirements.

## OODA

One of the most referenced military concepts in security is *OODA*, an acronym for observe, orient, decide, act. The OODA loop, shown in [Figure 2-1](#ch02.html#figure_2_1_the_ooda_loop), was developed by fighter pilot, military researcher, and strategist John Boyd in the 1960s. He believed that a fighter pilot who was at a disadvantage against an adversary with more-advanced equipment or capabilities could be victorious by using OODA to respond more quickly to stimuli and effectively attack the opponent’s mind through decisive actions.

![](assets/image-to-come.png)

Here’s an introduction to each of the four stages.

### Observe

The Observe phase centers around the collection of information. In this phase, an individual collects information from the outside world—anything and everything that could be useful. If the individual is planning to catch a baseball, this phase is about observing the baseball to determine its velocity and trajectory. If the individual is trying to catch a network attacker, the observation includes gathering logs, monitoring systems, and collecting any outside information that could help identify the attacker.

### Orient

The Orient phase puts the information collected during the Observe phase into context with already known information. This takes into account past experience, preconceived notions, expectations, and models. For the baseball example, orientation uses what the observer knows about how a ball moves, taking into account its velocity and trajectory, to predict where it will go and how much force the impact will generate when it is caught. In the example of a network attacker, orientation takes the telemetry pulled from the logs and combines it with knowledge about the network, relevant attack groups, and previously identified artifacts such as specific IP addresses or process names. The orientation phase is heavily reliant on the mental models we previously discussed. Without a way to quickly and accurately “sort” the data you have observed, orientation becomes a very difficult task.

### Decide

At this point, information has been collected (observed) and contextualized (oriented), and now it is time to determine a course of action. The decide phase is not about executing an action. It is about debating various courses of action until the final course of action is determined.

In the baseball example, this phase includes determining where to run and how fast, how the fielder should move and position her hand, and anything else needed to attempt to catch the ball. In the case of dealing with a network attacker, it means deciding whether to wait and continue to observe the attacker’s actions, whether to start an incident-response action, or whether to ignore the activity. In either case, the defender *decides* on the next steps to achieve their goal.

### Act

After all that, the Act phase is relatively straightforward: the individual follows through with the chosen course of action. This doesn’t mean it’s 100% guaranteed to be successful. That determination is made in the observation phase of the next OODA loop, as the cycle begins again back at the observation phase.

OODA is a generalization of the basic decision-making process that everyone goes through thousands of times a day. This explains how individuals make decisions, but also how teams and organizations do so. It explains the process a network defender or incident responder goes through when gathering information and figuring out how to use it.

The OODA loop is used by not only one side. While we, as defenders, go through the process of observing, orienting, deciding, and acting, in many cases the attacker is as well. The attacker is observing the network and the defender’s actions in that network, and deciding how to respond to changes in the environment and attempts to kick them out. As with many things, the side that can observe and adapt faster tends to win. [Figure 2-2](#ch02.html#figure_2_2_competing_ooda_loop_of_the_defender_and_the_attacker) shows the OODA loop for both an attacker and defender.

One thing to note about the OODA loop is that it can often trip analysts up as they try to think through adversarial responses to their actions - “first I will do x, then they will do y, and then I will…” - when in reality it can be difficult to know how the adversary will respond. There will always be some level of uncertainty when dealing with adversaries who are human and may act unpredictably, which can make the risk-adverse wary of making any moves. When in doubt, reach back to your requirements, or even further back to the goals and mission of your team or program. Are you tasked with defending the network? With safeguarding user data or public safety? Make sure those requirements and missions are included in the orientation phase, which can help to avoid decision-paralysis when it comes time to decide what the best course of action is.

![](assets/image-to-come.png)

This generalized decision model provides a template for understanding the decisions of both defenders and attackers. We’ll discuss the cycle more moving forward, but in the end, this model focuses on understanding the decision-making processes of all parties involved.

## Intelligence Cycle

The intelligence cycle, pictured in [Figure 2-3](#ch02.html#figure_2_3_the_intelligence_cycle), is the formal process for generating and evaluating intelligence. The cycle begins where the last intelligence process ended and continues to build off itself. The intelligence cycle doesn’t need to be followed to the letter. In fact, processes explored later in this book will build upon it. You do have to be careful not to omit critical steps, however. If you start skipping entire steps, you run the risk of ending up with more data and questions instead of intelligence.

![](assets/image-to-come.png)

To properly utilize the intelligence cycle, you need to know what is involved in the steps, which we will dive into next.

### Direction

The first step in the intelligence cycle is direction - also known as requirements. *Direction* is the process of establishing the question that the intelligence is meant to answer. This question can be delivered from an outside source, developed by the intelligence team itself, or developed by stakeholders and the intelligence team. (This is sometimes called the *RFI process*, which we’ll discuss in Chapter 4, Find. The ideal outcome of this process is a clear, concise question whose answer the stakeholders will find usable.

Much intelligence work is based around requirements. In the intelligence community, any work done or reports circulated needs to be tied directly to an intelligence requirement, either standing (long term) or priority (urgent and time sensitive) requirements.

### Collection

The next step is collection of the data necessary to answer the question. This is a wide-ranging exercise that should focus on gathering as much data as possible from many sources. Redundant information adds value here, because corroboration is often important.

This leads to a key idea of developing an effective intelligence program: building a collection capability. It’s difficult to know exactly what data might eventually prove useful, so building a broad capability to collect a wide variety of information is important. This includes tactical information such as infrastructure, malware, and exploits, as well as operational strategic information such as attacker goals, social media monitoring, news monitoring, and high-level document exploitation (identifying reports, such as those that vendors release about groups, and gathering information from them). Be sure to document the sources and take care: news stories often republish or reference the same original material, making it difficult to know what’s corroboration and what’s just a rehash of the same material. If it is impossible to determine the source of a particular data set, you may want to avoid using it as a collection source.

Collection is a process, not a one-time action. Using information from the first round of collection (such as gathering IP addresses) leads to a second round (such as using reverse DNS to find domains related to those IP addresses), which leads to a third round (using WHOIS to gather information about those domains). This exploitation becomes exponential as it builds upon itself. The focus at this point is not understanding how the data relates but simply developing as much information as possible. Combining it comes later. Also, don’t forget to consider internal sources, such as an incident-management system. It’s common for organizations to discover actors or attacks they’re already intimately familiar with.

### Processing

Data is not always immediately usable in its raw format or in the format in which it was collected. In addition, data from different sources may come in different formats, and it is necessary to get it into the same format so it can be analyzed together. The *processing* necessary to make data usable is often an overlooked task, but without it, generating intelligence would be nearly impossible. In the traditional intelligence cycle, processing is part of collection. However, when dealing with the types of data and organizations involved in incident response, it may be useful to consider processing separately. Here are some of the most common ways to process data related to cyber threats.

Normalization
: 

Processing includes normalizing collected data into uniform formats for analysis. The collection process will generate nearly every conceivable kind of data result. Intelligence data comes in a variety of formats, from JSON to XML to CSV to plain text from email. Vendors share information on websites in blog posts or tables, but also in PDF-based reports or even YouTube videos. At the same time, organizations tend to store data in different formats. Some organizations use a purpose-built threat-intelligence platform, while other organizations build customized solutions from wikis or internal applications.

Indexing
: 

Large volumes of data need to be made searchable. Whether dealing with observables such as network addresses and mutexes or operational data such as forum posts and social media, analysts need to be able to search quickly and efficiently.

Translation
: 

In some cases, regional analysts may provide human translation of source documents, but this is generally not feasible for most organizations dealing with information from all over the world. Machine translation, while imperfect, usually provides sufficient value so that analysts can find items of interest. If necessary, they can then be escalated to specialists for a more accurate translation.

Enrichment
: 

Providing additional metadata for a piece of information is important. For example, domain addresses need to be resolved to IP addresses, and WHOIS registration data fetched. Google Analytics tracking codes should be cross-referenced to find other sites using the same code. This enrichment process should be done automatically so that the relevant data is immediately available to analysts.

Filtering
: 

Not all data provides equal value, and analysts can be overwhelmed when presented with endless streams of irrelevant data. Algorithms can filter out information known to be useless (though it may still be searchable) and bubble up the most useful and relevant data.

Prioritization
: 

The data that has been collected may need to be ranked so that analysts can allocate resources to the most important items. Analyst time is valuable and should be focused correctly for maximum benefit to the intelligence product.

Visualization
: 

Data visualization has advanced significantly. While many analysts fear vendor dashboards because of the clutter they typically contain, designing a visualization based on what analysts need (rather than what marketing and executives think looks good) can assist in reducing cognitive load.

Taking the time to process data effectively enables and improves future intelligence efforts.

### Analysis

*Analysis*, as much an art as it is a science, seeks to answer the questions that were identified in the Direction phase. In intelligence analysis, data that has been collected is characterized and considered against other available data, and an assessment is made as to its meanings and implications. Predictions are often made as to future implications. There are various methods for conducting analysis, but the most common is to use analytic models to evaluate and structure the information, identify connections, and make assessments about their implications. In addition to preexisting models, which we cover later in this chapter, it is also common for analysts to develop their own models that work with their particular data sets or way of interpreting information.

The goal of the Analysis phase is to answer the questions identified in the Direction phase of the intelligence cycle. The type of answer will be determined by the nature of the question. In some cases, the analysis may generate a new intelligence product in the form of a report or could be as simple as a yes/no answer, most often backed up with a confidence value. It is important to understand what the output will be before beginning the analysis.

Analysis is not a perfect science and must often be conducted with incomplete information and uncertainty. It is important that analysts identify and clearly state any information gaps in their analysis so that decision makers can be aware of potential blind spots in the analysis. Information gaps can also drive the collection process to identify new sources in order to reduce those gaps. If the gaps are significant enough that an analyst does not think it is possible to complete the analysis with the current information, then it may be necessary to go back to the Collection phase and gather additional data. It is much better to delay the final analysis than to provide an assessment that the analyst knows is flawed.

It is important to note that *all intelligence analysis is generated by a human*. If it is automated, it is actually processing instead, which is a critical step in the intelligence cycle but is not by itself analysis.

### Dissemination

At this point, the process has generated real intelligence: a contextualized answer to the question posed in the Direction phase. A report with an answer is useless until it’s shared with the relevant stakeholders: those who can use this intelligence. In plenty of documented intelligence failures, analysis was spot-on but dissemination failed. *Intelligence must be shared with relevant stakeholders in the form they find the most useful*. This makes dissemination dependent on the audience. If the product is aimed at executives, it’s important to consider length and phrasing. If it’s aimed at implementation in technical systems (such as IDS or firewalls), this could require vendor-specific programmatic formats. In any case, intelligence must be usable by the relevant stakeholders.

### Feedback

Often forgotten, the Feedback phase is key to continuing intelligence efforts. *The Feedback phase asks whether the intelligence that was generated answers the direction successfully*. This results in one of two outcomes:

Success
: 

If the intelligence process answered the question, the cycle may be over. In many cases, though, a successful intelligence process leads to a request for more intelligence based on either new questions or the actions taken based on the answer given.

Failure
: 

In some cases, the intelligence process failed. In this case, the Feedback phase should focus heavily on identifying the aspect of the original direction that was not properly answered. The following Direction phase should take special care to address the reasons for that failure. This usually comes down to a poorly structured Direction phase that didn’t narrow the goal enough, or an incomplete Collection phase that was unable to gather enough data to answer the question, or improper analysis that did not extract correct (or at least useful) answers from the data available.

## Using the Intelligence Cycle

Let’s consider how the intelligence cycle can be used to start learning about a new adversary.

One of the most common questions a chief information security officer, often abbreviated as CISO, asks (hopefully before she gets asked it herself) is, “What do we know about this threat group I heard about?” A CISO will want a basic understanding of a group’s capabilities and intention, as well as an assessment of relevance to a given organization. So what does the intelligence process look like in this situation? Here is an example of what is involved in each step of the intelligence cycle to meet the CISO’s needs:

### Direction

This came from a key stakeholder: the CISO. “What do we know about *X* threat group?” The real answer sought is a target package, which we’ll explore in detail later.

### Collection

Start with the original source, most likely a news article or report. That document will usually provide at least some context for beginning the collection. If the source material includes information about a specific intrusion or attack, it can be helpful to understand more about the entity that had been targeted and identify any potential motivations or goals of the intrusion. If indicators (IPs, URLs, etc.) exist, explore those as deeply as possible by pivoting and enriching. The source may itself point to additional reporting with IOCs, tactics, techniques, and procedures (TTPs), or other analyses.

### Processing

This is very workflow/organization dependent. Getting all the collected information into a place where it can be used most effectively may be as simple as putting all the information into a single text document, or it may require importing it all into an analysis framework. Additional enrichment can be done with technical details related to the group. In addition, translation of reports or other documents may be necessary.

### Analysis :

Using the collected information, the analyst will start by attempting to answer key questions:

*   What are these attackers interested in?
    
*   What tactics and tools do they typically use?
    
*   How can defenders detect those tools or tactics?
    
*   Who are these attackers? (Although this is always a question, it is not always one worth taking the time to answer.)
    

### Dissemination

For a product like this that has a specific requester, the CISO, a simple email may suffice. Although in some cases limiting a response to this makes sense, a real product for proactive distribution to others will almost always create greater value.

### Feedback

The key question: is the CISO pleased with the results? Does it lead to other questions? These pieces of feedback help close the loop and may begin a new series of collections.

The intelligence cycle is a generalized model that can be used to answer questions large and small. However, it is important to note that following the steps will not automatically result in good intelligence.

# Qualities of Good Intelligence

The quality of intelligence relies primarily on two things: collection sources and analysis. Many times in cyber-threat intelligence we end up working with data that we did not collect ourselves, and therefore it is critical that we understand as much as possible about the information. When generating intelligence ourselves, we also need to ensure that we understand collection sources and are addressing biases in our analysis. Here are some things that should be considered to ensure that quality intelligence is produced:

Collection  method
: 

It is important to understand whether the information is collected primarily from incidents or investigations, or whether it is being collected from an automated collection system such as a honeypot or a network sensor. Although knowing the exact details of the collection is not imperative—some providers prefer to keep their sources confidential—it is possible to have a basic understanding of where the data comes from without compromising collection resources. The more details you have about the way information was collected, the better your analysis of this information will be. For example, it is good to know that data comes from a honeypot; it is better to know that it comes from a honeypot configured to identify brute-force attempts against remote web administration tools.

Date of  collection
: 

The majority of cyber-threat data that is collected is perishable. The lifespan of that data varies from minutes to potentially months or even years, but there is always a period of time when this information is relevant. Understanding when data was collected can help defenders understand how it can be acted upon. It is difficult to properly analyze or utilize any data when you do not know when it was collected.

Context
: 

The collection method and date can both provide some level of context around the data, but the more context that is available, the easier it will be to analyze. Context can include additional details, such as specific activities related to the information and relationships between pieces of information.

Addressing biases in  analysis
: 

All analysts have biases, and identifying and countering those biases so that they do not influence analysis is a key component of quality intelligence. Some biases that analysts should seek to avoid include *confirmation bias*, which seeks to identify information that will support a previously formulated conclusion, and *anchoring bias*, which leads analysts to focus too heavily on a single piece of information while disregarding other, potentially more valuable information. We cover biases in depth in Chapter 7, Analyze.

# Levels of Intelligence

The intelligence models we have examined thus far focus on a logical flow of information through a sort of analysis pipeline. But just as with incident analysis, this approach is not the only way to model the information. We can think about intelligence at different levels of abstraction, ranging from the highly specific (tactical) to the logistical (operational) to the very general (strategic). As we examine these levels of intelligence, keep in mind that this model represents a continuous spectrum with gray areas between them, not discrete buckets.

## Tactical Intelligence

*Tactical intelligence* is low-level, highly perishable information that supports security operations and incident response. The customers for tactical intelligence include security operations center (SOC) analysts and computer incident response team (CIRT) investigators. In the military, this level of intelligence supports small-unit actions. In cyber-threat intelligence (CTI), this usually includes IOCs and observables as well as highly granular TTPs describing precisely how an adversary deploys a particular capability. Tactical intelligence enables defenders to respond directly to threats using methods such as hunting in the network for signs of adversary activity, prioritizing critical patching based on reports of active exploitation, or issuing information to employees of active phishing campaigns targeting their organization.

An example of tactical intelligence is IOCs related to an exploitation of a newly discovered vulnerability. These tactical-level IOCs include IP addresses conducting scans searching for the vulnerability, domains hosting malware that will be downloaded to the host if exploitation is successful, and various host-based artifacts that are generated during exploitation and installation of malware. This tactical intelligence would enable security operations teams to effectively hunt for malicious activity in the network while taking concrete steps to mitigate any future exploitation.

## Operational Intelligence

In the military, *operational intelligence* is a step up from tactical. This information supports logistics and analyzes effects of terrain and weather on larger operations - in other words, it involves far more context than just the tactical mission at hand. In CTI, this usually includes information on campaigns and higher-order TTPs, as well as anticipated responses from adversaries and how those actions may impact other operations. It may also include information on specific actor attribution as well as capabilities and intent. Customers for operational intelligence include senior-level Digit Forensics and Incident Response (DFIR) analysts and other CTI teams.

This is one of the harder levels of intelligence for many analysts to work with, because it sometimes feels too general to be tactical but too specific to be strategic. Operational intelligence needs to be acted on somewhat urgently, however there are potentially far reaching implications of any action taken, so both the “observe” and “orient” phases of the OODA loop have far more components and complexities. In addition, the necessity of making a decision and acting on it is felt differently by analysts who are working on the data and decision makers, over at the executive level, who may want to spend a considerable amount of time weighing the best course of action.

Following the preceding example about tactical-level indicators of active exploitation of a vulnerability, operational-level intelligence would include information on how widespread the exploitation is, whether it is targeted or opportunistic, who else is being targeted, the purpose of the malware that is being installed, and any details on the actors who are carrying out the attacks. Understanding these details can support the generation of follow-up intelligence, including what other actions that may be seen, and should include information on the severity of the threat to help plan a response.

## Strategic Intelligence

In the military or government, *strategic intelligence* deals with national and policy-level information and is often the accumulation of years of analytic work aimed at providing a holistic picture of a situation. In CTI, we think of this as supporting C-level executives and boards of directors in making serious decisions about risk assessments, resource allocation, and organizational strategy. This information includes threat trends and actor motivations, along with additional information relevant to the organization. In the preceding example, strategic intelligence would include information on the motivations of the attackers, especially if the activity indicates a new or previously unidentified threat, and any information that indicates new tactics or attacker targeting that may require higher-level responses, such as new policies or an architecture change.

# Confidence Levels

As mentioned previously, intelligence typically has different confidence levels associated with it. These confidence levels reflect the analysts’ trust that the information is correct and accurate. For some types of data, this confidence may be on a numeric scale (for example, 0 to 100) and calculated using traditional statistical methods, while in other cases the confidence assessment is provided on a qualitative, subjective basis by analysts directly. It is important to identify confidence in two important areas: confidence in the source of the information, and confidence in an analyst’s conclusions.

One common way of describing source reliability is the Admiralty Code or NATO System found in [FM 2-22.3](https://fas.org/irp/doddir/army/fm2-22-3.pdf). This consists of two scales. The first evaluates the reliability of a source based on previous information, ranging from A (Reliable) to E (Unreliable). The second scale evaluates the degree of confidence in the information content itself, ranging from 1 (Confirmed) to 5 (Improbable). These two scores are combined for a particular piece of information based on the source and specific content, so that information known to be true from a source with a history of valid information might be evaluated as B1, but information that is improbable from a source with a history of invalid information would be evaluated as E5.

Sherman Kent, often referred to as the father of intelligence analysis, wrote an essay in 1964 called “Words of Estimative Probability,” which describes various qualitative ways to describe confidence in an analyst’s judgment. In that essay, Kent shares one of the charts that he and his team use to assign and describe confidence (shown in [Figure 2-4](#ch02.html#figure_2_4_sherman_kents_chart_on_estimative_probability)) but also writes that other terms may be used in their place as long as the meaning is understood and the terms are used consistently.

![](assets/image-to-come.png)

# Conclusion

Intelligence has a long and fascinating history and is a field that has adapted time and time again to changes in technology and to the growing complexities of the world. At its foundation are principles that are part of human nature - curiosity, assessing situations for danger, making connections. These foundations have been built upon with structured processes and models that make these often instinctive mental processes more academically rigorous and defensible. Biases, which humans cannot completely escape, can influence analysis and need to be identified and countered in analysis to avoid inaccurate assessments.

Intelligence is also a critical component of incident response, and many processes can be used to integrate intelligence principles into incident response investigations. It is important to understand the sources of intelligence that you will be relying on; there is a big difference in the way that you treat intelligence that came from a previous IR investigation in your network and the way you treat information that comes from a honeypot. Both types of information are valuable; they just have different applications. The next chapter dives into the specifics of incident response and the models that help analysts implement intelligence-driven incident response.

^([1](#ch02.html#idm46530565004560-marker)) https://www.history.com/news/russia-georgia-war-military-nato

# Chapter 3. Basics of Incident Response

> “It is a fairly open secret that almost all systems can be hacked, somehow. It is a less spoken secret that such hacking has actually gone quite mainstream.”
> 
> Dan Kaminsky

Intelligence is only one half of the intelligence-driven incident-response puzzle. While computer incident response isn’t nearly as old as the art of espionage, in the last 40 years it has rapidly evolved into a major industry. *Incident response* encompasses the entire process of detecting intrusions (whether against a single system or an entire network), developing the information necessary to fully understand them, developing and executing the plans to remove the intruders, and recording information for follow up actions (such as legal actions, regulatory reporting, or intelligence operations).

Intrusion detection and incident response share many characteristics. Both are abstract. Both complicated topics, and as a result people have sought to simplify them by abstracting them into cycles or models. These models make understanding the complex interplay between defender and adversary possible and form the basis for planning how to undertake responding to these incidents. Just like intelligence models, they are rarely perfect and can’t always be followed explicitly, but they provide a framework for understanding the adversaries’ intrusion and the defenders’ response processes and allow multiple responders to work together to understand an intrusion using the same terminology and methodology. The combination of shared vocabulary and thought processes helps make these complex events less fraught and far more likely to succeed.

Just like the exploration of intelligence, this chapter starts with overarching theory and then moves to more specific applications and models. Afterward we’ll dig into common defensive techniques and finish with the integrated intelligence and operations models we’ll use for the rest of the book.

# Incident-Response Cycle

In the same way that we need a standard language for discussing intelligence concepts, we need a language to discuss detecting and responding to incidents. This process can be viewed from both the defender’s perspective and the adversary’s perspective. Let’s start with the defenders.

The *incident-response cycle* is made up of the major steps taken in intrusion detection and incident response. The goal of this model is to be agnostic to the type of attack (phishing, strategic web compromise, service compromise, etc.) and generalize the steps that are generic to all of those attacks (and many others). [Figure 3-1](#ch03.html#fig_1_the_incident_response_cycle) illustrates this cycle.

![](assets/image-to-come.png)

There’s some argument about where the concept of the incident-response cycle began. The first references seem to come from the National Institute of Standards and Technology’s seminal document on incident response, “NIST 800-61 Computer Security Incident Handling Guide ([http://bit.ly/2tGfwUz](http://bit.ly/2tGfwUz)).” Now in its second revision, this document is the basis for how United States Government agencies handle incidents. Although this guide introduced a huge number of key concepts, one of the most important is the description of the incident-response cycle, which provides a defender centric view of the incident-response process. This process includes five phases:

*   Preparation
    
*   Identification
    
*   Containment
    
*   Eradication
    
*   Recovery
    

Each phase of the cycle serves a specific purpose and is critical to the success of the entire cycle. It is important to understand the purpose of the entire cycle, along with the details of each phase, before beginning the process. Because of this, we will go in depth into each of these phases below.

## Preparation

From the defender’s perspective, the first stage of an incident comes before the attack begins: the *Preparation phase*. Preparation is the defender’s chance to get ahead of the adversary by hardening assets, deploying detection capabilities, creating and updating signatures, and understanding baseline system and network activity. This is a combination of network security architecture and security operations. Many of these steps go beyond the security team, impacting and being impacted by general network operations, network architecture, system management, and even help desk and support.

Preparation should focus on five key elements, three technical and two nontechnical:

Attack Surface Mapping
: 

It’s extremely difficult to defend systems that are unknown. In today’s world of shadow IT, cloud providers, and rapid prototyping it’s nearly impossible to have a complete understanding, let alone inventory, of every system in a network. Yet without this understanding, or as close as possible, defense remains a guessing game.

Telemetry
: 

You can’t find what you can’t see. Specialized systems are required for incident responders to identify and investigate intrusions. These systems range from network to host and should provide the ability to investigate a wide variety of activities at multiple levels.

Hardening
: 

The only thing better than identifying an intrusion quickly is it never happening in the first place. Preparation is the stage for ensuring that patches are deployed, configurations are locked down, and tools that limit attacks such as virtual private networks (VPNs) and firewalls are in place.

Process and documentation
: 

On the nontechnical side, process is the first line of defense that can be prepared ahead of time. Few things are worse during an incident than trying to figure out what you’re doing as you’re doing it. Along with processes (such as an incident-response plan, notification plan, and communications plan), having documentation for common questions such as network configurations, system configurations, and system owners will also speed up responses.

Practice
: 

The last thing preparation allows is the chance to practice your plans. This will speed up future incidents and identify issues that can be corrected (something we’ll touch on more in “Lessons Learned”). The best incident-response teams are those that have been through incidents together, and the best way to do that is practice.

When discussing computer network defense, many (mostly pessimistic) people are quick to point out all the advantages an adversary has. Most of these advantages boil down to surprise: in computer network exploitation, the adversary gets to pick the location and time of the attack. What many don’t take into account is the key advantage of the defender: namely, the ability to prepare for an attack by controlling the environment. The adversary can do reconnaissance, but in many cases is attacking a black box, without full understanding of the target until the attack is underway. Defenders can leverage this by preparing adequately and maximizing their “home field” advantage.

## Identification

The *Identification phase* is the moment where the defender identifies the presence of an adversary impacting their environment. This can occur through a variety of methods:

*   Identifying the adversary entering the network, such as a server attack or an incoming phishing email
    
*   Noticing command-and-control traffic from a compromised host
    
*   Seeing the massive traffic spike when the adversary begins exfiltrating data
    
*   Getting a visit from a special agent at your local FBI field office
    
*   All network devices suddenly displaying a screen announcing you’ve been hit with ransomware.
    

Regardless of what actions the adversary takes, the Identification phase begins when you first gain awareness of adversary activity against your resources. In this model, the Identification phase is the entire intrusion-detection phase, glossing over many details in a complicated topic (we’ll dig into other models specifically about this phase later). This is obviously an oversimplification, but reasonable given that this cycle focuses on the overarching, end-to-end process of incident response rather than digging into the details of each phase. The Identification phase typically leads to an investigation, discovering even more information about the incident and the adversary, before beginning to respond directly. One of the key goals of threat intelligence is to augment the Identification phase, increasing the accuracy and quantity of methods to identify adversaries earlier.

Identification, at least in the incident-response sense, is not simply hearing about an attack that took place or learning about a new adversary. Identification starts when there is direct impact to your users, systems, or resources. For it to be an incident, there has to be impact.

On the other hand, if an adversary has capability, intent, and opportunity, that adversary does represent a threat. This isn’t the start of the incident cycle, but the intelligence cycle. Only after the adversary is identified in your environment should an incident begin.

## Containment

The first two phases of the cycle can be considered primarily passive and are focused on information gathering. The first phase of actual response, meaning that specific actions are being taken in response to a specific attack, is containment. *Containment* is the initial attempt to mitigate the actions of an adversary, stopping the bleeding in the short term while preparing a longer-term response. These shorter-term responses may not make the attack impossible, but they dramatically reduce the ability of the adversary to continue to achieve the objectives. These actions should be taken in a rapid but controlled manner to limit the adversary’s opportunity to respond.

Common containment options are as follows:

*   Disabling the network switch port to which a particular system is connected
    
*   Blocking access to malicious network infrastructure such as IPs (at the firewall) and domains or specific URLs (via a network proxy)
    
*   Temporarily locking a user account under the control of an intruder
    
*   Disabling system services or software an adversary is exploiting
    

In many incident responses, defenders may choose to skip over the Containment phase entirely. Containment risks tipping off the adversary by changing the environment while that adversary still may have control. Skipping directly to eradication may also give comfort to executives who see containment alone as a band-aid.

## Eradication

*Eradication* consists of the longer-term mitigation actions meant to remove an adversary from an environment and keep them out for good, unlike the temporary measures in the Containment phase. These actions should be carefully planned out and may take a considerable amount of time and resources to deploy. Eradication actions are focused on completely removing the adversary’s ability to regain access to the network once they are kicked out. It includes focusing not just on the specific tactics that allowed them to gain access in the first place, but also additional hardening of the network based on the knowledge of the adversary’s objectives and Tactics, Techniques, and Procedures (TTP).

Common eradication actions are as follows:

*   Removing all malware and tools installed by the adversary (see the sidebar “Wiping and Reloading Versus Removal”)
    
*   Resetting and remediating all impacted user and service accounts
    
*   Re-creating secrets that could have been accessed by the adversary, such as shared passwords, certificates, and tokens
    
*   User education and awareness about the incident to reduce future social engineering
    
*   Patching software vulnerabilities and changing vulnerable configurations
    

Often responders will go for a *scorched-earth* approach to eradication. In these cases, responders will take remediations on resources with no indications of compromise; for example, regenerating all VPN certificates after an adversary accessed one VPN server. Scorched-earth approaches are effective at mitigating the *unknown unknown* situations, where it’s impossible to know 100% what the adversary did, but comes with the compromise that it may require a significant effort to make these changes.

The effort necessary varies based on the sort of service or information involved. Forcing full password resets in an active directory–managed Windows environment is relatively easy. Regenerating and redeploying extended validations (EV) TLS certificates with domain pinning in major browsers is hard. The incident-response team needs to collaborate with the corporate risk management and system/service owner teams to determine how far to go in these situations.

For example, in the spring of 2015, Pennsylvania State University took its entire engineering network offline for three days in response to a compromise ([http://bit.ly/2u7R5Bu](http://bit.ly/2u7R5Bu)). Afterward it had to bring the network back online and return it to normal service. A recovery like this requires removing malware from systems, resetting credentials such as passwords and certificates, patching software, and many other changes meant to completely remove the adversary from the network and limiting that adversary’s ability to return. In this case, the mitigation action, taking the entire network offline (likely to limit the adversary’s ability to make changes during the remediation phase), preceded the remediation actions. This is a common pattern when dealing with persistent adversaries. On one hand it decreases the adversary’s ability to react to the mitigation and remediation (except by pre-planned or automated means) but on the other hand it gives a clear signal to the adversary that a response is under way and does cause a significant negative impact to anyone using the network.

## Recovery

Containment and eradication often require drastic action. *Recovery* is the process of going back to a non-incident state. In some regards, recovery is less from the attack itself, but more from the actions taken by the incident responders.

For example, if a compromised system is taken from a user for forensic analysis, the Recovery phase includes returning or replacing the user’s system so that the user can resume their business functions. If an entire network is compromised, the Recovery phase involves undoing any actions taken by the adversary across the entire network, and can be a lengthy and involved process, sometimes including rebuilding the entire network. Cloud services add a variety of pros and cons; on one hand they can be difficult to track down shadow IT, on the other in many cases these networks are deterministic (built using configuration management tools) that can make resetting and rebuilding easier.

This phase depends on the actions taken during the prior two phases, the adversary’s methods, and the resources that were compromised. It generally requires coordination with other teams, such as desktop administrators and network engineering.

Incident response is always a team sport, requiring actions by a wide variety of security and non-security teams, but in no part of the incident is this quite as obvious as during recovery. Security may set certain requirements for the way systems are recovered (most of these will take place during eradication), but recovery, after the incident-response team’s all clear, is handled largely by IT and system owners. Figuring out how to work together and collaborate effectively is key and often requires both practice and experience (though good documentation also helps). Few things can compromise a response faster than IT beginning recovery before the IR team has fully eradicated the threat.

## Lessons Learned

The last phase of the incident cycle, akin to many other security and intelligence cycles, includes taking time to assess past decisions and learn how to improve in the future.

This phase can be called many things, an After Action Report, Hot Wash, or Retrospective (Retro). The Lessons Learned phase, no matter what it’s called, evaluates the team’s performance through each step of the incident. Basically, this takes the incident report and answers some basic questions:

1.  What happened?
    
2.  What did we do well?
    
3.  What could we have done better?
    
4.  What will we do differently next time?
    

As an exercise, this can often be daunting. Many teams resist reviewing lessons learned or conducting after-action reviews. This occurs for a wide variety of reasons, from being concerned about mistakes being highlighted (and thus blamed on the IR team) to simply not having enough time due to new or ongoing incidents. Whatever the reason, nothing will keep an incident response team from advancing like skipping lessons learned. The goal of the Lessons Learned phase is to discover how to make the next incident response go faster, smoother, or ideally never happen at all. Without this crucial step, IR teams (and the teams they collaborate with) will make the same mistakes, suffer from the same blockers, and fail to identify improvements.

Although it’s important, conducting a Lessons Learned phase doesn’t have to be daunting; in fact, it should be the opposite. A good after-action doesn’t need to take hours or require everyone involved in the incident response. Here are a few detailed starter questions you will want to ask when evaluating each phase during the Lessons Learned process:

Preparation
: 

*   How could we have avoided the incident altogether? This includes changes to your network architecture, system configuration, user training, or even policy.
    
*   What policies or tools could have improved the entire process?
    

Identification
: 

*   What telemetry sources (IDS, net flow, DNS, etc.) could have made it easier or faster to identify this attack?
    
*   What signatures or threat intelligence could have helped?
    

Containment
: 

*   Which containment measures were effective?
    
*   Which were not?
    
*   Could other containment measures have been useful if they’d been more easily deployable?
    

Eradication
: 

*   Which eradication steps went well?
    
*   What could have gone better?
    

Recovery
: 

*   What slowed the recovery? (*Hint:* focus on communication, as that’s one of the toughest parts of recovery to do well.)
    
*   What did the response to recovery tell us about the adversary?
    

Lessons Learned:
: 

*   Not to be too meta, but even evaluating how the Lessons Learned process could be done more effectively; for example, would it help if responders took notes throughout the process? Did you wait too long and things were lost or forgotten?
    

Lessons Learned can also be practiced (the same as any other piece of the incident-response process). Don’t just do Lessons Learned for actual incidents, but take the time to build and follow your Lessons Learned process for red team and tabletop exercises as well. In fact, building lessons learned into table top exercises is essential.

Ultimately, the key to Lessons Learned is having the understanding that although early lessons learned will be painful, they will improve—and that’s the point. Early Lessons Learned exercises will call out flaws, missing technology, missing team members, bad processes, and bad assumptions. Growing pains with this process are common, but take the time and gut through them. Few things will improve an IR team and IR capability as quickly as some tough lessons learned sessions. In addition, capture these lessons and share them with your leadership and related teams. Although it seems like calling out a team’s flaws, in many cases these reports provide concrete justification for making changes that will improve your incident-response capability.

The incident-response cycle is one of the first models that incident responders learn for good reason: it succinctly describes the life cycle of an investigation. Where it becomes key is by taking the time to evaluate your team’s ability to execute at each stage, from Preparation through to Lessons Learned. As with intelligence work, there are several models and frameworks that can help in the incident-response cycle.

# Kill Chain

Another military concept that has made its way into cyber threat intelligence vernacular is the *kill chain*. In fact, it has become so popular that finding information on the original kill chains is difficult because of the extent of information security use cases and marketing. While this concept was on the fringes for years, a paper by Lockheed Martin researchers Eric M. Hutchins et al. titled [“Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains”](http://lmt.co/2miXqrZ) brought the concept into the information security mainstream with a formalized mapping of the most common intrusion pattern as a formalized kill chain.

Since their report, the kill chain has become a go-to model for cyber threat intelligence, referenced by nearly every vendor, and a staple guiding process of defensive teams. The kill chain provides an important abstraction for the phases an adversary moves through when exploiting a target, particularly scenarios that involve phishing-based attacks.

But what is a kill chain? In its simplest form, a *kill chain* is a series of steps an adversary must conduct in order to achieve an objective (see [Figure 3-2](#ch03.html#fig_2_cyber_kill_chain)). In our case, we’re discussing a computer network adversary, but it works for many adversarial activities. This means abstracting the incident process, but whereas the incident cycle is focused on the *defender’s* actions, the kill chain focuses on the *adversary’s* actions.

![](assets/image-to-come.png)

The kill chain is a good way to abstract an adversary’s TTPs. It provides a structure to understand the abstract actions of an attack.

Though the earlier phases of the kill chain, especially Targeting and Reconnaissance, are sometimes opaque to the defender, usually difficult and circumstantial to detect, they’re worth understanding. Defenders tend to think that adversaries succeed every time with zero effort. This is far from the truth. In fact, our ability to disrupt these pre-intrusion phases later might be one of the strongest advantages defenders have.

## Targeting

Before the kill chain proper begins, the adversary must decide what to attack (for example, who is the potential target). In many cases, this occurs in consultation with some form of sponsor or stakeholder, often as part of their own intelligence or operational requirements. As defenders, we commonly think of the target as the victim organization, but in many cases that is less important than the information or capabilities being targeted.

Targeting is an interesting aspect of the kill chain because it speaks to motivation and thus the general category (though not necessarily specific identification or attribution) of an adversary. For example, an adversary who wants to steal money needs to go where the money is. Understanding what’s being targeted gives insight into what the adversary is after (the adversary’s ultimate goals) and may lead to better defensive techniques (discussed later in “Active Defense”).

## Reconnaissance

After deciding *what* and *who* to target, the adversary begins conducting reconnaissance. In the *Reconnaissance* phase (or simply *recon*), the adversary develops as much information as possible about the planned victim. Reconnaissance can fall into multiple categories based on the type of data sought (hard data versus soft data) and collection methods (passive versus active).

### Hard data versus soft data

The intelligence world has multiple ways of dividing information based on subject (SIGINT, TECHINT, etc.) as we discussed in Chapter 2, but for computer network operations, we can think of things a bit more simply.

*Hard data* includes information about technical aspects of a network and the systems attached to it. For an adversary (and defenders while investigating adversaries), this usually includes open source intelligence:

*   Footprinting or enumerating the target network
    
*   DNS information such as reverse DNS
    
*   Operating systems and application versions
    
*   Information about system configurations
    
*   Information about security systems
    

*Soft data* includes information about the organization behind the network and its systems:

*   Organizational charts, public relationships, and other hierarchy documents
    
*   Business plans and goals
    
*   Hiring information, which can often leak information such as technologies in use
    
*   Information on employees, both professional and personal, for use in social engineering attacks
    

### Active versus passive collection methods

Adversaries may use different methods of collecting information. We can categorize these methods as *active* or *passive*:

*Active methods* require interacting directly with the target. An active hard intelligence collection could be port scanning a system directly. An active soft collection could be a social engineering attack to gather information about internal hierarchy and contact information.

*Passive methods* are based on collecting information without interacting directly with the target, often by gathering information from a third-party information service such as DNS or WHOIS. A passive hard collection might exploit domain information from a public service. A soft passive collection gathers information about an organization from a site such as LinkedIn, where people often share considerable information (even information they shouldn’t).

A defender’s ability to detect this recon activity varies tremendously. Active methods are much easier to detect than passive methods, for example, and most network defenders have more control over hard information than soft information. Detecting active hard collection in the form of a port scan is easier than detecting soft passive collection in the form of reading job postings for positions that describe specific technologies in use at an organization. In some cases, adversaries can proxy or bounce their reconnaissance activity through public services, some of which execute active methods such as network scans or DNS interrogation at the request of a user. This level of anonymity or hiding in noise benefits defenders and adversaries alike, as many such tools are used by both sides.

## Weaponization

If all security controls, or all software in general, worked as intended (that is, as their designers imagined, not as they were built), then adversaries would almost always fail. Thus, the goal for adversaries is to find places where the intention and implementation don’t match—a vulnerability. This vulnerability must then be exploited reliably and packed into a form that’s ready to be delivered to a target (for example, a malicious document or exploit kit). The process of finding this vulnerability, crafting an exploit, and combining it with a payload is *Weaponization*.

### Vulnerability Hunting

The vulnerability subphase of weaponization is particularly interesting given its effect on what targets can be attacked. This forces the adversary to make a decision. Some widely deployed pieces of software are likely to be found in any environment, such as Adobe Acrobat and Reader or Microsoft Windows and Office. This means that any exploit targeting them is widely usable. However, these pieces of software have been attacked for years, and their respective companies have made considerable efforts to identify and mitigate vulnerabilities. The alternative is attacking a more esoteric piece of software that may be less defended but also less widely deployed. This limits where an adversary can use it. This process can be tied in with the cyber kill chain’s *Reconnaissance* phase. The effort that adversaries are willing to put in may also be influenced by their own direction and intelligence requirements.

As an example of this trade-off, consider the Stuxnet incident, in which unconfirmed adversaries disabled centrifuges within the Iranian nuclear facility at Natanz. Part of this included deploying exploits against programmable logic controllers (PLCs) in Siemens equipment. While this equipment is not widely deployed in most organizations, it existed in the target environment. Vulnerabilities in those PLCs thus provided a vector for the adversaries to carry out their mission.

We, as defenders, disrupt this process constantly by using development-centric security approaches. Good development practices such as [Microsoft’s Security Development Lifecycle](https://www.microsoft.com/en-us/sdl/) reduce the introduction of the mismatches that become vulnerabilities. Application security teams and related reviews constantly hunt for these vulnerabilities in source code. Strong patch management can help eliminate old vulnerabilities within an environment.

Every vulnerability that is patched constrains adversaries a little bit and forces them to find new vulnerabilities to exploit. This is a time-consuming and expensive process. The longer a vulnerability is in the wild, the more value it has to an adversary because of its longer effective life span. In turn, then, disrupting the return on investment (ROI) of a vulnerability brings defensive value.

Imagine that an adversary has a privilege escalation in Windows 95. The adversary uses it for years, and it finally gets fixed in Windows 7. This means the adversary could use that exploit for multiple years across multiple versions. The longer it lasts, the more ROI the adversary receives from the vulnerability hunting and exploit creation effort.

That same adversary later finds an Internet Explorer 11 code execution vulnerability and uses it in a series of attacks. However, after three months, defenders find and patch the vulnerability. The adversary had less time to get ROI out of that vulnerability, requiring him to go back to the drawing board and find a new one. This requires additional resource allocation for a smaller window of effectiveness.

### Exploitability

A vulnerability is just a crack in the armor. It takes an actual exploit to take advantage of that crack. The exploitability process is all about finding a method to trigger the vulnerability and turn that into actual control of program execution. Much like vulnerability hunting, this phase may have its own specialists or occur in conjunction with other exploit phases. This is a topic unto itself, addressed well in the book [*Hacking, The Art of Exploitation*](https://www.nostarch.com/hacking2.htm) by Jon Erickson (No Starch Press, 2008).

After an exploit is crafted, the adversary must make it reliable. This can be complicated because exploits don’t always work, given things like language packs and specific defenses such as Microsoft’s Enhanced Mitigation Experience Toolkit (EMET) or Address Space Layout Randomization (ASLR) for Linux. Further, exploits that crash target code or systems will draw attention.

An exploit, though, simply opens the door and gives the adversary a method to access the target (or at least an intermediate target). For the next step, the adversary will need an implant.

### Implant Development

Generally, the goal of an exploit includes delivering some sort of payload for the adversary to then use to further their goals (such as data exfiltration). The implant will allow the adversary to maintain access to the exploited system without having to continually exploit the device, which can be noisy, and if the system is patched, it would remove the possibility of exploitation at all. As a result, implant development follows many of the same processes as traditional software development, with an emphasis on stealth (to avoid detection) and capabilities (to allow adversaries to achieve their objectives). Thus, if an adversary wants to be able to listen to conversations within range of a compromised system, the implant needs the ability to activate the microphone, record what it hears, and transmit the resulting audio files, all without triggering suspicion from the user or any security software that’s running.

There are two primary types of implants. The first is a beaconing implant that calls out to a command-and-control server and will receive commands to be carried out on the target system. The second is an implant that does not beacon, but waits to receive a command and then begins to communicate with a command-and-control server. Implant development is often determined by the network topology and device type. Sometimes a previously developed implant can be used, but in other situations an adversary will need to develop something specifically for the network that is being targeted.

Although many computer network operations still lean heavily on the need for an adversary to keep persistence and install capabilities with implants, a growing number of actors seek to achieve their objectives without installing any implants. The compromise of Hillary Clinton campaign chairman John Podestas email was conducted without ever deploying an implant at all, just by stealing his password. In many ways, this style of attack is more difficult for investigators because without an implant, there’s one fewer artifact to analyze. Implantless attacks are another case where understanding the adversaries’ goals will help contextualize their techniques.

### Testing

Both exploits and implants then go through extensive testing as a part of the Weaponization phase. Much like software development, testing could be little more than a spot check, or it could mean extensive testing conducted by separate quality assurance teams. For malicious code, the testing phase focuses on two aspects: *function* and *detectability*.

The functionality aspect is much like any other software development project; the testing team needs to ensure that the software does what it’s designed to do. If it’s meant to steal files, the implant must be able to read from the filesystem on the target host; find the correct group of files; usually bundle, encrypt, and compress them; and then exfiltrate them to a system the adversary controls. This may seem easy, but there are a wide variety of variables that the development team may not always be able to control, and thus need to be tested for.

The detectability aspect is unlike anything seen in normal software development. Testing teams will attempt to verify that their software is undetectable by security tools they might see in their target environment, such as antivirus or other endpoint software. This ties directly into the functionality aspect as many heuristics-based security systems look for certain behaviors that the malicious code may need to achieve its objective, such as setting registry keys to maintain persistence. These detectability requirements may be based on assumptions, or for especially hard targets, based on information gathered during reconnaissance.

### Infrastructure development

While not strictly a part of the Weaponization phase, infrastructure development is another key preparation task an adversary needs to complete before the attack. Most attacks rely on pieces of infrastructure to support the malicious code deployed onto a victim’s machines. Command-and-control servers are needed to direct attack actions. Exfiltration points are needed to upload and then retrieve stolen data from. Hot points are needed for adversaries to pivot through to obfuscate their real location if their other infrastructure gets compromised. Adversaries need a wide variety of infrastructure to execute the various phases of their operation:

Certificates
: 

First of all, it’s important to differentiate the two most common types of certificates used in intrusions.

*   Code signing certificates, provided by operating system builders like Apple and Microsoft, are necessary to run new applications without bypassing security controls. Without code signing certificates users often have to go through multiple steps to execute an unsigned program. These certificates can be difficult to get, especially fraudulently (companies work hard to make this the case).
    
*   Transport Layer Security (TLS) certificates encrypt communication with websites, keeping web traffic private. Without these certificates most browsers will throw an error, warning the user their communication is insecure. Even worse is a mismatch between the certificate and the domain, which will throw a more severe error. These certificates used to be very difficult to get but thanks to services like LetsEncrypt they’ve become far easier recently.
    

Adversaries want both code signing and TLS certificates for modern attacks to avoid these browser and operating system level protections. Some adversaries instead try to encourage users to bypass security controls, but this can be risky both because users are more likely to not bother and there’s a higher chance the user might realize something seems off (and hopefully report it to their intrepid security team).

Servers
: 

Like most internet-based activities most adversaries rely on servers at one or many stages of their attacks. This includes delivery, either to host a downloaded file (for a link in a phishing email) or to host a browser exploit. In later stages servers can enable communication for command and control or as a target to upload stolen files or credentials. These may be adversary owned and run (bare metal) but, like so many organizations many adversaries have moved to using cloud servers or even directly using public web services. Both of these have the advantage of blending in.

Domains
: 

Few network connections go directly to IP addresses, so most adversaries use domain names. Some domains are chosen as specific lures or confuse analysts, meant to mimic or seem similar to other recognizable domains (such as using homography attacks such as using a lowercase L in place of a capital I).

Email Addresses
: 

Phishing is the first, and most obvious usage of email by adversaries. These email addresses are usually built to be throw away or limited use and like domains may emulate related domains or even specific people. Email addresses are also required to set up other infrastructure, to put in certificates, and even to associate with payment sending and receiving methods like cryptocurrency.

## Delivery

Once the adversary has gathered enough information to craft an attack, the next kill chain stage is Delivery. Common delivery scenarios include but are not limited to the following:

Spear Phishing
: 

The adversary sends a weaponized exploit, either as an attachment or as a link, via direct communications (often email) to a specific target. The communication is usually crafted to appear legitimate and reduce suspicion in the mind of the targeted user. In recent years adversaries have also leveraged instant messaging platforms and SMS to send communications with a malicious link to users.

Service Exploitation
: 

A common group of vectors in the early 2000s direct exploitation of web facing services has reared its head again. In these cases, adversaries find a vulnerability in a web facing service, such as a web application or virtual private network, and attack that directly. In many cases these are commodity systems (such as Chinese-nexus adversaries compromising Pulse VPN - [https://www.mandiant.com/resources/suspected-apt-actors-leverage-bypass-techniques-pulse-secure-zero-day](https://www.mandiant.com/resources/suspected-apt-actors-leverage-bypass-techniques-pulse-secure-zero-day)). While some of these attacks are highly targeted, vulnerabilities like Shellshock and Log4J have been widely exploited.

Strategic Web Compromise (Watering Hole)
: 

In a two-stage attack, often based on a previous service exploitation scenario, the adversary first compromises a secondary resource, usually a website the intended target is likely to use, and places a browser or media exploit on it. The assumption is that the target, usually focused at an entire group rather than a specific individual, will visit the site, becoming compromised.

The key to delivery is how simple it is: it’s just getting the payload to the victim. This simplicity belies the importance of this stage. Delivery is the first necessarily active stage on the part of the adversary to the victim. While the previous stages can (in the case of Targeting and Reconnaissance) be active, Delivery is the first case where an adversary *must* be active, which often results in something the victim (either the intended target or the security team) could detect. This means delivery is the first case where a victim is guaranteed to have indicators of compromise and adversary tactics, techniques, and procedures to build from. In the case of spear phishing, this may be email artifacts such as headers and an email address, while for SQL injection it may be an IP address that initiated the web server/database connections.

## Exploitation

Understanding the difference between delivery and exploitation can be challenging. Up through the start of delivery, the adversary has not had direct interaction with the target and does not have any control of the targeted system. Even in the case of a spear phishing email, it is possible that security measures will prevent successful delivery, so even though there was delivery, there was no actual exploitation. Exploitation is the point where the adversary’s gain control of code execution and begin executing their own code.

In a watering hole attack, this takes place the second a victim hits an infected page. For the spear phishing attack, this is when the victim clicks on a malicious attachment or link. From this point forward, the adversary has control of at least one process on the target’s system. This foothold is the start of the adversary’s move into the network.

Exploitation is a massive topic unto itself to the point it has its own specialists (and indeed its own companies, market places, and even economy). For a deeper understanding take a look at:

*   [The Shellcoder’s Handbook - 2+ Edition](https://www.goodreads.com/book/show/1174511.The_Shellcoder_s_Handbook?ac=1&from_search=true&qid=bOCHnNGKPA&rank=1) by Jack Koziol, Chris Anley, and John Heasman
    
*   [The Ida Pro Book](https://www.goodreads.com/book/show/3004460-the-ida-pro-book?from_search=true&from_srp=true&qid=SrY3kR5hSP&rank=1) by Chris Eagle
    
*   [Real World Bug Hunting](https://www.goodreads.com/book/show/35545851-real-world-bug-hunting?from_search=true&from_srp=true&qid=DF9uclWR3D&rank=1) by Peter Yaworski
    
*   [A Guide to Kernel Exploitation](https://www.goodreads.com/book/show/9224826-a-guide-to-kernel-exploitation?from_search=true&from_srp=true&qid=LMXQzQLd7c&rank=1) Enrico Perla and Massimiliano Oldani
    

## Installation

Getting initial execution is often difficult and not something an adversary wants to do multiple times. Once adversaries have code execution, their first move is typically to solidify their access by creating a permanent foothold. The Lockheed Martin Kill Chain paper describes this in the following way: “Installation of a remote-access Trojan or backdoor on the victim system allows the adversary to maintain persistence inside the environment.” While this is what the adversary usually does at this stage, we find it useful to look at these actions as establishing system or network persistence (in many cases, the adversary will do both, but it still helps to consider them separately).

### System persistence

At this point, the adversary has code execution on a single system, and likely just a single process at that. This is a useful start, but it doesn’t persist past a reboot. A shutdown of the compromised application may even remove their access.

Most adversaries begin by solidifying their hold on a small number of hosts by deploying a rootkit or remote-access trojan (RAT) style of implant. A rootkit establishes kernel-level access to a system and, once installed, permits an adversary to evade many detection methods of the underlying OS. A RAT is a piece of remote-control software meant to persist past reboots and without relying on a certain exploit. This allows the adversary to persist on an individual host.

### Network persistence

The vast majority of adversaries aren’t content to establish a single-system foothold. Instead, they want to establish deeper persistence. To do so, they’ll typically establish a wider footprint, using one (or both) of two techniques:

Establish system persistence on multiple systems
: 

This means using captured credentials and installing RATs or similar access methods on other systems. An adversary has a variety of options for this, from custom software to native tools such as PsExec in Windows or SSH in \*nix environments.

Gathering credentials that allow access to broadly utilized network resources without accessing a system on the network
: 

This often means VPNs, cloud services, or other internet-exposed systems such as web mail. This lowers the risk of detection in many cases and doesn’t require a form of malware, instead using native tools.

These techniques can be used individually or together. This combination of techniques is often a signature set of techniques for most adversaries.

## Command and Control

Once an adversary has established persistence, especially if they’ve chosen the RAT route, they need a method to deliver commands (either via push or pull). Command and Control (C2) communication are carried out using a variety of methods and can leverage different ports, protocols, and services. Adversaries have been known to be incredibly creative in setting up C2 processes. One notable example is Turla, a group known to get innovative with command and control channels. In one instance they used an implant that communicated via encoded commands on popstar Britany Spears’ Instagram comments. In the past, many pieces of malware, especially distributed denial-of-service (DDoS) tools, communicated by joining IRC channels or HTTP calls to a server under the adversary’s control. The [Comment Crew](http://symc.ly/2tHeLKX) got its moniker from doing command-and-control with HTML comments on otherwise innocuous-looking web pages. Some adversaries use multiple methods, including DNS lookups, social media, or popular cloud applications.

Adversaries focus on making sure that their communication channels avoid notice and provide enough bandwidth to meet the needs of the adversary. In some cases, malware may communicate using only a few lines of text a day, while others include full virtual desktop capability.

## Actions on Objective

In most cases, all of this is not the ultimate goal, but rather the setup. Adversaries go through the process of setting up access in order to give themselves the capability to affect the target in a way they didn’t have before. We call this new capability the *actions on objective (AoO)*.

While many of the earliest AoO were as simple as exploration, since the mid 1990s the most common action on objective has been collecting and exfiltrating information on target systems. In a very simple way, it makes a lot of sense: computers are ultimately about processing information. Supposedly infamous bank robber Willie Sutton was once asked why he robbed banks. Sutton responded “Because that’s where the money is.” In the modern age where money is data (as ones and zeros in a ledge or blockchain), plans for the next military technology are in computer aided design and development files on corporate servers, and political intelligence is stored as PDFs or Excel spreadsheets is it any wonder data theft became the dominant action on objective?

Next to data exfiltration the most common action on objective, and goes along with the network persistence piece of installation, is pivoting. In many cases the true target an adversary is after isn’t available to attack directly. People shouldn’t be opening phishing emails from the FTP server that holds their most valuable plans (they often do unfortunately) and shouldn’t be browsing shady websites from the system with all their customer data (they do that too) but in the cases where these practices are followed the adversary will often attack related systems that may facilitate further access to get to the ultimate target. In many cases the adversary wouldn’t even know what system they ultimately targeted, but would attack anything available, establish persistence, and pivot; essentially starting the entire intrusion process over again (kill chains within kill chains). A common intrusion pattern has been for the initial system to be quite innocuous and unrelated to anything (such as the adversary targeting a recruiter or accountant, who’s likely to have Microsoft Office macros enabled), get admin credentials by cracking the Security Account Manager (SAM) file, pivot to the Domain Controller, steal the SAM file once again. Now the adversary has network wide administrator credentials, and can go after their ultimate target directly.

Aside from information theft and pivoting the most common actions on target by attackers, not just on computers, were categorized by the US Air Force as follows (We’ll talk more about these a bit later on in regards to what defenders can do to respond to adversaries):

Destroy
: 

The adversary destroys a physical or virtual item. This could mean destroying data, overwriting or deleting files, or otherwise making a system unavailable until it is completely rebuilt. This could also mean destroying a physical object, though this is a rare occurrence in computer attacks, for example the Stuxnet destruction of Iranian centrifuges.^([1](#ch03.html#idm46530557019536))

Deny
: 

The adversary denies usage of a resource (such as a piece of infrastructure or a given capability) by the target, such as in the case of denial-of-service attacks that do not permit access to a site. Another example that has gained attention in recent years is ransomware, which encrypts a user’s data and requires payment before the adversary will (in theory) decrypt the data for use again.

Degrade
: 

The adversary degrades the utility of the target’s pieces of infrastructure or capabilities. This most often refers to the target’s ability to control and command infrastructure, such as disabling networked services. A slightly less nuanced version of this, with a simple killchain, is distributed denial of service.

Disrupt
: 

By interrupting the flow of information, an adversary can disrupt the target’s ability to carry out normal operations.

Deceive
: 

The adversary seeks to cause the target to believe something that is not true. In this context, the adversary may be inserting false information into a workflow to redirect assets or information, or cause the target to take a course of action to the adversary’s benefit.

Most of these are straightforward and basic on the surface. However, the manner in which an adversary puts these together is key and often speaks directly to the adversary’s identity and goals. An adversary can often hide malware, obfuscate command and control, and so forth; but ultimately the actions on the objective cannot be obfuscated, encoded, or protected. To steal information, an adversary must steal the files. To execute a DoS, an adversary must use compromised hosts to send massive amounts of network traffic. In short, the Actions on Objective phase can’t be faked.

It’s also important to understand that an adversary may combine multiple actions and vectors, including physical/non-cyber actions. This could include anything from recruiting an insider to leak strategic information to a *kinetic action* (for example, bombing a geographic site).

As you can see in [Figure 3-3](#ch03.html#fig_3tk), the adversary’s kill chain stays tightly defined while the incident cycle has to react to it, which starts in the Identification phase. Identification can occur anywhere between the Identification and the Actions on Target phases of the kill chain, and causes dramatically different incident responses to occur. An incident identified during the Delivery phase is ideal. The defenders can block the attack at an email or web proxy and keep the attack from ever executing. An attack detected during a later phase, such as Command and Control or Actions on Objective, is likely going to be painful, involving many compromised resources and an expensive and lengthy incident-response investigation.

![](assets/image-to-come.png)

## Example Kill Chain

To illustrate the kill chain, imagine a series of attacks by a fictitious attack group codenamed Grey Spike. This group conducts political intelligence collection against multiple countries. Most recently, reporting indicated that they have been targeting election campaigns in the US and UK. They seek to gain information about candidates’ stances on economic, foreign policy, and military issues. Here’s what their strategy would entail:

Targeting
: 

Grey Spike does not choose its own targets but receives tasking (intelligence jargon for what data and organizations they’re instructed to target) from national policy makers. This tasking describes specific target countries and candidates as well as key words of interest.

Reconnaissance
: 

Grey Spike begins operations by gaining an understanding of the network footprint of its targets, including domain names, mail servers, key technologies, and web and mobile applications. The group also gathers information on key personnel, such as campaign management, social media managers, and the technology consulting firms retained by the campaigns.

Weaponization
: 

Grey Spike receives quarterly resource allocations including zero-day vulnerabilities, but in general prefers to use these when no other vector is available. In this case, the actor has a suite of download macros that it implants as payloads in documents custom-written in the target languages by regional and cultural specialists on loan from other departments within their intelligence agency. Additionally, infrastructure in the form of private servers used for C2 and delivery is rented in the name of shell companies from providers around the globe.

Delivery
: 

The operators send the weaponized documents to key managers in the campaign staff. Each document is written to appeal to that manager specifically, including offers to provide financial contributions and endorsements. Because of the rapid pace of the campaign trail, the targets open these documents with a high success rate, allowing the adversary’s implants to run on their laptops.

Exploitation
: 

The implant code runs in the form of a document macro that runs an older exploit for the PDF reader used by the campaign. Although a patch has been available for the vulnerability for some time, campaign staff have frozen all updates because of the belief by senior staff that this may cause outages at inopportune times.

Delivery
: 

The exploit code is a downloader that then contacts a malware delivery server in a shared hosting environment at a popular ISP to install a remote access Trojan (RAT) on the target system. The RAT next contacts a C2 server at a bulletproof ISP in a third country.

Command and control
: 

Grey Spike issues commands to the RAT via the C2 channel, in this case through encoded DNS lookups. Using their covert channel, they conduct searches for the target’s email and relevant documents. In addition to some of the information they’ve been tasked with acquiring, they find emails documenting shared accounts, including passwords that they can then use to extend their access throughout the network.

Actions on Objective
: 

In this scenario, Grey Spike is tasked specifically with information retrieval only. The policy makers do not wish to interfere directly with the campaign, largely because of concern about political consequences, despite having the technical ability to destroy most of the data and online infrastructure of the candidates.

Kill chains help to organize incident-response data in a way that allows you to visualize what the attack looked like as it took place, and can help identify patterns in activity. Another method to accomplish this is the diamond model, which we will discuss next.

# Diamond Model

The diamond model ([Figure 3-4](#ch03.html#fig_3_the_diamond_model)) for intrusion analysis differs in many ways from the kill chain, though later in this section we’ll discuss how they complement each other. Originally discussed in a seminal paper by Christopher Betz, Sergio Caltagirone, and Andrew Pendergast, they summarize the model as follows: “An *adversary* deploys a *capability* over some *infrastructure* against a *victim*. These activities are called *events*...events are phase-ordered by an adversary-victim pair into *activity threads* representing the flow of an adversary’s operations.” Ultimately, it is a paradigm for understanding the interaction between the various actors (the adversary and the victim) and the adversary’s tools (infrastructure and capabilities).

![](assets/image-to-come.png)

The diamond model is an incredibly powerful tool for analyzing and understanding intrusions and it is worth it to understand how to use and extend the model in intrusion analysis.

## Basics of the Diamond Model

An ***adversary*** is an intelligent party (actor) with intent to gather information or do harm to an information system. We can further break this down into the *operator* carrying out the actions and the *customer* who will benefit from the actions. The same person might fulfill both roles (in a small mercenary or financially-motivated adversary group), but not always (as in the case of a nation-state SIGINT agency where the customer and operator could come from different agencies, let alone teams). Adversaries will have an intent, which could be as general as “make money through credit card fraud” or as specific as “acquire the communications of a specified person regarding a particular topic of interest.”

An adversary has a set of exploits and techniques, called a ***capability***, they can deploy to accomplish their goals. This includes the weaponized software and implants discussed in the kill chain, but it can also include the ability to carry out a social engineering attack or, in some cases, deploy some sort of physical ability, whether for gathering information or disrupting systems.

An adversary uses a set of communication systems and protocols, called ***infrastructure***, to deliver or control a capability, or to cause some result on a victim. This includes systems owned directly by the adversary (e.g., desktops and network equipment in their physical possession) and systems compromised and re-purposed (e.g., end-user systems in a botnet).

Adversaries target a ***victim*** against which they deploy their capabilities in the attempt to accomplish a certain intent. The victim includes people and assets, and both may be targeted separately. And as noted, a victim system may then be used as infrastructure against another victim.

Each occurrence in which all four elements are present (an adversary using a capability across infrastructure against a victim) represents a single event. Events that feed into each other then are analyzed as *activity threads*, and we can further collect those threads into *activity groups* (related threads that may run in parallel or otherwise not necessarily flow linearly).

## Extending the Model

One of the ways in which the model starts to demonstrate its power comes from looking at the axes across the diamond. The “north-south” axis connecting adversaries and victims represents a social-political relationship. The adversary has some interest in the victim, whether as a class (credit card numbers) or specifically (a CEO targeted for spear phishing to enable fraudulent wire transfers). Analysis of this axis can shed light on an adversary’s motivation and possibly assist with attribution or operational and strategic planning for intrusion detection and incident response.

As mentioned at the beginning of this section, the diamond model complements the kill chain, and in fact the two integrate well. Each event in the diamond model can be categorized according to its phase in the kill chain. This allows the analyst to have a greater understanding of the relationship between events and consider investigating and documenting phases that you have previously neglected to investigate. (That said, not every phase of the kill chain will be present in every incident.)

This model has also been extended to act as a clustering mechanism, where analysts attempt to compare multiple kill chains together to find similarities and group them into campaigns and actors (something we’ll discuss later on.

The Diamond Model for intrusion analysis is a well-used reliable method for analyzing adversary activity, however it leaves the format and the method for capturing different information up to the user of the model In recent years, methods for standardizing and capturing information on adversaries tactics and use of infrastructure. Two of these frameworks, ATT&CK and D3FEND, have essentially become industry standards. Whether combined with the Diamond Model or used independently, these frameworks can be incredibly useful in the intrusion analysis process.

# ATT&CK & D3FEND

The MITRE Corporation (usually just called MITRE), a United States Federally Funded Research and Development Center, has released a variety of useful cyber frameworks that can be used independently or in conjunction with other models. These two models have different focuses, one on helping understand and categorize adversary actions, and one of focused on counter measures that can be used by the defense. Both of these frameworks are worth exploring in depth, however here we will provide a brief overview focusing on how these methods can augment other models useful in the IDIR process.

## ATT&CK

The Kill Chain and the Diamond Model for Intrusion Analysis are two examples of models that you are likely to use frequently, whether explicitly or as mental models. Another more recent addition to the analyst’s toolkit is the MITRE ATT&CK Framework, which enhances and extends the traditional kill chain. ATT&CK stands for “Adversary Tactics, Techniques, and Common Knowledge”(and yes, if you’re thinking it, no one likes the Ampersand in the middle, but these are the things you do for good acronyms). MITRE describes ATT&CK as “a globally-accessible knowledge base of adversary tactics and techniques based on real-world observations \[which is\] used as a foundation for the development of specific threat models and methodologies”.

ATT&CK can be broken down into a few key groups of major concepts (Taken directly from attack.mitre.org):

| Concept || MITRE Definition |
| --- | --- |
| Tactics || Tactics represent the “why” of an ATT&CK technique or sub-technique. It is the adversary’s tactical goal: the reason for performing an action. For example, an adversary may want to achieve credential access. |
| --- | --- |
| Techniques || Techniques represent ‘how’ an adversary achieves a tactical goal by performing an action. For example, an adversary may dump credentials to achieve credential access. |
| Groups || Groups are sets of related intrusion activity that are tracked by a common name in the security community. Analysts track clusters of activities using various analytic methodologies and terms such as threat groups, activity groups, threat actors, intrusion sets, and campaigns. |
| Software || Software is a generic term for custom or commercial code, operating system utilities, open-source software, or other tools used to conduct behavior modeled in ATT&CK. |

There are a few other Secondary categories as well, including matrices, data sources, mitigations, and resources. Matrices are important as they tie together other data into various environments. The vast majority of the time people are referring to the ATT&CK Enterprise matrix, but Mobile and ICS (as in Industrial Control System) could also be important depending on your organization. Data Sources is focused on mapping data back to the collection source, i.e. the sensor or type of log you can find the data in. Mitigations, which are split up by matrix, represent ways to respond to various attacks as well.

The tactics and techniques captured in ATT&CK are not theoretical - they have been observed by cybersecurity practitioners while tracking adversaries conducting network operations. These tactics and techniques are captured as individual components that are displayed in a matrix form. ATT&CK is a multi-functional model. It can be used as a library of tactics and techniques to help an incident responder and intelligence team build their understanding of existing and emerging adversary trends.

At this point ATT&CK has moved beyond the US government, or even being a mental model for cyber threat intelligence analysts and incident responders, and is now in use by vendors who base their detections around ATT&CK, offensive security (we’ll talk more about Red Canary Atomic Red Team later on), technology evaluators, and even ambitious authors. We’re going to discuss more about ATT&CK throughout the rest of this chapter and even the rest of this book.

## D3FEND

MITRE also released the other side of ATT&CK; D3FEND (and another on the nose yet delightful acronym). Created with funding from the National Security Agency, D3FEND enumerates the countermeasures and responses defenders can take to block or respond to an adversary. Similarly, to ATT&CK D3FEND splits these into tactics (the overarching goals) and techniques (the technical controls actually being implemented). The goal of D3FEND is to tie specific defensive techniques to specific offensive techniques. Between these two models it *should* be possible to go from a specific actor (say G0032 - Lazarus Group) all the way to the specific system hardening steps you should take to block them and the type of detections you need to find them.

Will this work? It might be too soon to tell. ATT&CK was slow to get started for the first few years, so it’s possible D3FEND will gain speed. This ends up being a great lead into our next topic.

# Active Defense

One of the most talked about and least understood concepts about intelligence-driven incident-response cycles is the idea of active defense.

Active Defense is frequently equated with the idea of “*hack back”*, attempting to attack a malicious actor directly. Although this qualifies as one aspect of active defense, five other useful pieces of active defense are far more common. This mix-up is based on a fundamental misunderstanding of the purpose of active defense.

Most people who attempt or request some form of hack back have in mind a childish, schoolyard-style form of revenge. It’s natural. You get hit; you want to hit someone back. There are reasons we’ve outgrown these juvenile tactics. First of all, in network intrusions, we often find it difficult to know the adversary’s identity, leading to misattribution and thus a misdirected attack. It can be hard to know if a given technology asset is the property of an adversary, something they’re purchasing from a vendor (yep, adversaries love the cloud as much as your devops team), a free service that the adversary is leveraging, or even a stolen asset (meaning an attack might not just be against an adversary, but the innocent legitimate owner). Second, proportionate response is difficult for defensively oriented organizations. Third, it usually serves a limited purpose aside from a sense of revenge and retribution. Fourth, hacking back could, depending on the adversary, cause them to retaliate to the hack back. Lastly, and perhaps most importantly, it is illegal in most countries to attempt to compromise a computer system without its permission or appropriate legal authority such as granted to law enforcement and military organizations, which in theory submit to strict oversight. In short you probably can’t do it.

###### Note

Aside: You might sit there and say “But what about the people who do have the authority and information to hack back? Well, it turns out that’s starting to happen. In late 2021 Gen. Paul Nakasone, head of Cyber Command and director of the National Security Agency, speaking in the New York Times^([2](#ch03.html#idm46530557114624)), acknowledged that “we have taken actions and we have imposed costs” against ransomware operators. How effective this will be long term is yet to be seen.

Apart from hacking back, however, active defense includes several other valid and useful elements. Wendi Rafferty at the SANS DFIR Summit in 2015 described the goal of active defense as attempting to disrupt the tempo of an adversary. Andrew Thompson of Mandiant uses the same phrasing as Gen. Nakasone, describing it as “imposing cost”. Like a detective, the goal of incident responders is to catch an adversary in a mistake, providing a chance to expose them. Active defense gives defenders a means to accelerate that by forcing the adversary into an error, generally while responding to a roadblock put into place by the incident-response team.

As previously discussed for adversaries, defenders have the option to deny, disrupt, degrade, deceive, and destroy (Thus we call this model the D5 model of defense). Originally developed as a series of desired capabilities for computer network attack (CNA), it turns out the same D5 model provides a great list of capabilities for active defense.

## Deny

The idea of denying an adversary is so straightforward and common that most organizations wouldn’t even imagine it’s a type of active defense, most are actively doing deny style actions regularly. If we go by our traditional definition of disrupting the adversary’s tempo, though, this is a perfect example. Denying can be simple, such as implementing a new firewall rule to block an adversary’s command and control, applying a system patch for a vulnerability, or shutting down access for a compromised email account. The key to denial is *preemptively excluding a capability or infrastructure from the malicious actor*.

Denial forces adversaries to deviate from their plan and to find a different way to achieve their objectives. If the adversaries don’t change every single indicator of compromise (IOC) before continuing, you can force them into revealing TTPs and pivot your investigation to their new activities. Additionally, many deny actions could be interpreted as mere coincidence, such as a user resetting a password because of a required window, not at the direction of the incident-response team.

## Disrupt

If the Deny action preemptively excludes a capability or infrastructure from the malicious actor, then Disrupt *actively excludes a resource from the malicious actor*. In most cases, disruption requires active observation of an adversary in order to know when they’re active so that they can be disrupted in real time. This could mean cutting off a command-and-control channel while it’s being used or interrupting the exfiltration of a large archive file.

## Degrade

Closely related to disrupting and denying an adversary, Degrade focuses on *marginal reduction of an adversary’s resources while they’re actively being used*. An easily understandable example is throttling an adversary’s bandwidth during exfiltration, causing a large file to upload over an extremely slow time frame. This degradation of access attempts to frustrate adversaries, hopefully driving them to attempt to access the data in a different way and expose additional infrastructure, tools, or TTPs.

Disruption and degradation present interesting but dangerous opportunities for a network defense team. While denial actions may be explained away as coincidence or normal passive defensive operations, disruption and degradation are clearly active. They begin to open up a dialogue with an adversary, giving the adversary an indication that the defender is deliberately responding. Adversaries in situations like this can take a variety of actions. They may spool up attack tempo and severity by bringing advanced capabilities to bear, or they may go all the way in the other direction and cease operations entirely while they wait for the heat to go down. This is the risk of these types of active defense, and they need to be taken with a measure of caution and preparation.

## Deceive

Easily the most advanced of available techniques, the Deceive active defense action is based on the counter intelligence concept of *deliberately feeding adversaries false information* with the hopes they’ll treat it as truth and make decisions based on it. This ranges from planting false documents with incorrect values to hosting honeypot systems or even networks.

Deception operations require a deep understanding of the adversary’s goals, methods, and even psychology, as well as your own resources. Making deception material that an adversary is willing to accept as truth is incredibly difficult, as skilled adversaries will attempt to corroborate any material they find with other sources. That said, deception has gotten the attention of vendors selling tools ranging from simply honeypots or tokens (a passive deception approach), all the way up to advanced deception suites that mimic entire networks and monitor adversaries as they pivot around (active deception).

## Destroy

Destroy actions do actual harm, whether kinetic or virtual, to an adversary’s tools, infrastructure, or operators. In most cases, this is the purview of law enforcement, intelligence, or military operators (in the United States these are Title 10 and Title 50 organizations) that have the legal authority to commit such acts. For a commercial or private organization to do so is not only generally accepted to be illegal but also dangerous. It is unlikely that they have the tools, methodology, and operators to conduct a successful computer network attack, and there could be significant unintended consequences. Those resources are better allocated to improved defensive operations.

Is active defense for you? Active defense is a trendy topic, but should organizations make it a part of their security program? As with almost any simple question about complex topics, the answer is, “It depends.” Active defense doesn’t have to be implemented in its entirety. Denying an adversary is in the realm of any organization, and in fact most are probably already doing it whether they know it or not. Disruption and degradation require maturity. Deception is an advanced tactic that carries high reward but also high risk and should be reserved for all but the most advanced security teams. As we just said, the Destroy active defense action requires special legal status that’s not available to non-government organizations.

# F3EAD

The last major cycle to cover combines the intelligence generation aspects of the intelligence cycle with the operations-centric aspect of the incident and kill chain cycles. Built as a targeting methodology for special operations teams working with intelligence analysis cells in the field, F3EAD addresses two key issues with both of the previous cycles:

*   Intelligence cycles shouldn’t just lead to more intelligence: they should lead to meaningful operations. In our case, this means threat intelligence shouldn’t just lead us to more threat intelligence but instead to aggressive incident-response actions.
    
*   Operations cycles shouldn’t end after the objective is completed. The information gained during any operation should start feeding a new intelligence cycle. In our case, when an incident response is concluded, the information developed during it should be fed into the intelligence apparatus to start developing new intelligence, learn from previous incidents, and be better prepared for future intrusion attempts.
    

Thus, these two cycles, operations and intelligence, *feed into each other* instead of just themselves ([Figure 3-5](#ch03.html#fig_4_the_f3ead_operations_intelligence_cycle). Each incident-response operation leads to intelligence operations, and each intelligence operation leads to an incident-response operation, continuing the cycle.

![](assets/image-to-come.png)

To facilitate this process, F3EAD^([3](#ch03.html#idm46530557116704)) uses a modified version of a combined intelligence and operations cycle: Find, Fix, Finish, Exploit, Analyze, Disseminate. As you’ll see, this means going through the incident-response cycle and feeding the results into the intelligence cycle, and then connecting those results back into a new incident-response cycle.

## Find

The *Find* phase includes the *targeting* phase of the operation, which is where you determine the threats that you will address. This can come from many sources, such as intelligence from a vendor or open source. Ideally, your own previous intelligence cycles should feed this process as well. Depending on the operation or investment, this phase may be determined by the IR team itself, in conjunction with the intelligence team, or even working with groups outside these teams, such as the SOC or management. This parallels the Preparation phase of the incident-response cycle.

## Fix

Based on the information from the Find phase, the *Fix* phase establishes telemetry and determines where an adversary is on the network as well any external presence we can detect. Just to be clear, *fix* does not mean to repair (we were confused too!); it refers to the identification of the operational presence of the adversary within the network. This involves taking available information and figuring out which systems, services, or resources an adversary may have compromised, what their channels of communications are, and how they’re moving around your network. We can think of this as the Identification phase of the incident-response cycle.

## Finish

The *Finish* phase includes the actual incident-response action (as opposed to the more kinetic and lethal actions intended for the original military version of this process—please do not do these). This is when you take decisive action against the adversary, carrying out the containment, mitigation, and eradication phases of the incident-response cycle.

The major transition in F3EAD is that the conclusion of the IR operation doesn’t end the cycle. Whether the organization shifts resources and responsibilities between teams or the same team changes its own activity focus, the end of the Finish phase starts the beginning of the Exploit phase. The intelligence half of the F3EAD process then begins.

## Exploit

The *Exploitation* phase roughly maps to the collection phase of the intelligence cycle, with one primary difference being that this phase is laser-focused on information gathered from the adversary during the operational phases of the cycle. Some of the information that is especially useful for intelligence-driven incident response includes:

*   Any indicators of compromise, including IP addresses, URLs, hashes, and email addresses
    
*   Automated enrichment for the IOCs (such as getting reverse DNS for given IP addresses or gathering WHOIS data)
    
*   Exploit delivery samples
    
*   Malware samples
    
*   Common vulnerabilities and exposures (CVE) and exploits
    
*   User and incident reports
    
*   Communications from the adversary
    
*   Previously identified TTPs
    
*   Adversary objectives, goals, and motivations
    

It’s impossible to list everything that might be useful, but analysts should collect as much information as they can about the various phases of the attack. Think through the kill chain and try to collect information about each stage where possible.

## Analyze

The Analyze phase maps to (surprise) the Analyze phase of the intelligence cycle. During this phase, the idea is to assess what the collected information is telling us about both short term and long term implications on the activity. This is achieved through a few methods:

*   Summarizing tactics, techniques, and procedures
    
*   Putting data into models or frameworks for further analysis
    
*   Interpreting timelines and kill chains to derive actionable recommendations
    

Like the general intelligence phase, the Analyze phase itself is cyclical. Malware analysis may lead to more IOCs, which themselves can be enriched, and may lead to finding more malware. The overall goal is to develop a complete picture of the actor and their tactics, techniques, and procedures, with a focus on how to detect, mitigate, and remediate their actions.

## Disseminate

Intelligence is only useful when it gets to the right audience in the right format, with enough time for it to be absorbed and actioned. Dissemination is not just pushing information out - it is identifying what information is needed by decision makers, delivering it in a way that they can consume, and being available to answer questions or accepted follow-up requests for information. Dissemination of intelligence is focused primarily on the audience, not on how the individual analyst prefers to operate. Because of this it is critical to understand the audience, which typically fall into one of the following categories:

Tactical
: 

The most immediate audience for the intelligence is the incident-response team, feeding back into the beginning of the next F3EAD cycle. They’ll want a focus on IOCs and summarized TTPs.

Strategic
: 

It generally takes only one significant incident for management to start having a strong investment and interest in the IR and threat intelligence teams. Their interest will be in highly generalized TTPs (more focused on campaigns than individual incidents) and actions on target. This intelligence should be useful to decision makers in future resource allocation and larger business planning (for example, informing risk assessments).

Third party
: 

Many organizations participate in some form of threat intelligence sharing group. Each organization has to determine its own rules of engagement for participating. Work with your leadership and legal teams to determine the best methods to do that. This may include intelligence from any level of abstraction, depending on your goals and appetite for collaboration.

Regardless of the level of intelligence or the audience you are addressing, you will want the information you disseminate to be clear, concise, accurate, and actionable.

## Using F3EAD

F3EAD is one of the most powerful concepts that can be implemented to improve both the threat intelligence and incident-response side of security operations. It’s also one of the most difficult. It’s hard to explain why an arcane Special Forces acronym that most people don’t even know how to say should be a part of an IT department’s standard operating procedures, but it provides a key missing piece: how operations can lead to better intelligence, and vice versa.

When thinking about what this looks like, rather than focus on the details, focus on the overall concept: security operations and incident response become an input to threat intelligence, and threat intelligence becomes an input to security operations and incident response. Whenever any part of the security operations team (whether that’s a SOC, CIRT, or individual engineers) completes an incident response, all of their outputs and documentation, notes, forensic artifacts, malware, and research should be passed on to the intelligence team. From there, the intelligence team exploits and analyzes this information. Their output based on that incident should be provided back to the ops team, and then the cycle continues. This ends up forming a kind of security operations/threat intelligence OODA loop – Observe, Orient, Decide, Act, The faster the security ops team can use this intelligence, the faster they can complete their operations tasks, which in turn drives more intelligence.

These operations + intelligence model doesn’t need to be limited to the SOC and intelligence teams. This same process can be used with the vulnerability management and application security (AppSec) teams. For example, when the application security team finds a new vulnerability, that vulnerability can then be treated as a piece of intelligence. Nothing guarantees the application security engineer was the first person to find this vulnerability. Thus, the AppSec team feeds as much information as possible to the SOC, which begins looking for any indications of previous attacks against that vulnerability.

# Picking the Right Model

The purpose of models is to provide a framework that can be used to interpret information and generate intelligence. Keep in mind the quote by George Box: “All models are wrong, some are useful.” Hundreds of models are available for intelligence analysis, and more useful ones can be pulled from other disciplines, such as military conflict theory and law enforcement^([4](#ch03.html#idm46530557100800)). Some of these models are meant to be general purpose, and some have been developed for individual or specific use-cases. When deciding what model to use, there are several factors to keep in mind. The time available for analysis can help determine which model is appropriate. If there is time to conduct an in-depth analysis or an incident, the diamond model for intrusion analysis may work well. If you have time constraints, something like the OODA loop can be used to drive decisions. The type of information may also dictate which model is most appropriate, as some models are designed to be used with certain data sources such as netflow or endpoint data. Finally, it may all come down to analyst preference. If an analyst has found that a certain model works well within their processes, then by all means continue to use that model. There may even come a time when the best option is to develop a new model.

# Scenario: ROAD RUNNER

Now that we’ve worked through many of the key models, you’ll need to understand intelligence-driven incident response. We’re about to launch into the practical nuts and bolts of incident response, cyber threat intelligence, and how they can work together to help you defend your organization.

The rest of the book is laid out using the joint operations/intelligence model F3EAD augmented with additional analytic models and frameworks that are especially well suited for intelligence-driven incident response. As we work through this process, we’ll use our own investigation into Road Runner, which is the name we have given to the election-focused activity we first learned about earlier in the chapter. We believe that this is the most recent tasking of the actor Grey Spike, and that they are actively targeting our organization as well as others. Using this adversary and related activity, we will move through the F3EAD process

Find
: 

The next chapter introduces how we target adversaries, both proactively and reactively, to gather the information that we need to support the rest of the F3EAD process. We will evaluate what we know about Road Runner both from previous incidents in our organization, along with information shared from external parties.

Fix
: 

This phase can be thought of as the investigation phase, which involves tracking an adversary in the victim’s environment. Here we will actively hunt for Road Runner activity in our networks, and who knows, we may even find signs of additional adversaries we didn’t know about before!

Finish
: 

Once all adversary activity has been identified, the Finish Phase removes an adversary from the environment.

Exploit
: 

Once the incident-response process is over, we’ll start developing data that resulted from our incident response process so that we can derive lessons and insight. We already know that the Road Runner campaign is being carried out by a well-funded and persistent adversary, we will need to actively work to prevent them from regaining access to our information. That starts with understanding as much as we can about their activities.

Analyze
: 

Next, we’ll develop that data into intelligence that can be useful for protecting our organization going forward and helping others who may be dealing with similar intrusions or the same adversary.

Disseminate
: 

After developing the intelligence, we’ll put it into a variety of useful formats for a variety of customers, so that the most possible use can be derived from the process.

By the end, we’ll have developed a full set of products, breaking down the activity known as Road Runner.

# Conclusion

Complex processes such as incident response often benefit from the use of models, which give the processes structure and can define necessary steps for task completion. Determining which models should be used depends on the situation, the data available, and in many cases the analyst’s preference. The more familiar you become with these models and their applications, the easier it will be to determine which models to use in response to different incidents.

Now let’s dive into Find!

^([1](#ch03.html#idm46530557019536-marker)) [https://www.wired.com/2014/11/countdown-to-zero-day-stuxnet/](https://www.wired.com/2014/11/countdown-to-zero-day-stuxnet/)

^([2](#ch03.html#idm46530557114624-marker)) [https://www.nytimes.com/2021/12/05/us/politics/us-military-ransomware-cyber-command.html](https://www.nytimes.com/2021/12/05/us/politics/us-military-ransomware-cyber-command.html)

^([3](#ch03.html#idm46530557116704-marker)) [https://smallwarsjournal.com/jrnl/art/f3ead-opsintel-fusion-“feeds”-the-sof-targeting-process](https://smallwarsjournal.com/jrnl/art/f3ead-opsintel-fusion-%E2%80%9Cfeeds%E2%80%9D-the-sof-targeting-process)

^([4](#ch03.html#idm46530557100800-marker)) [https://github.com/juanandresgs/papers/blob/master/VB2018-Guerrero-Saade-FINAL.pdf](https://github.com/juanandresgs/papers/blob/master/VB2018-Guerrero-Saade-FINAL.pdf)

# Chapter 4. Exploit

> “If you focus solely on the enemy, you will ignore the threat.”
> 
> Colonel Walter Piatt

At this point it is common for the final incident-response report to be delivered and the responders to move on to the next matter requiring their attention, but that is not where this book ends. Throughout the course of the investigation, incident response teams gather a lot of data on our attackers, look for additional information from within their networks, and take actions that have an impact on the attacker’s operations. Now we need to gather all of that data, analyze it for intelligence value, and integrate it into not only detection and prevention methods, but also more strategic-level initiatives such as risk assessments, prioritization of efforts, and future security investments. To get to the point where you can do all these things, you have to engage the intelligence portion of the F3EAD cycle: Exploit, Analyze, and Disseminate.

It is no secret why most security teams stop short of completing the F3EAD cycle: it’s hard enough to generate intelligence, but managing it is a whole new series of headaches. Dealing with processes, timing, aging, access control, and formats is enough to make anyone’s head spin. And yet, as undeniably complex as these problems are, they have to be addressed head on. Properly extracting and capturing information about an incident and ensuring that it is followed up on can mean the difference between truly remediating an adversary’s access to your network and simply delaying their return and eventual success. Exploiting the intelligence that you have generated during the incident-response process ensures that all of the time and energy that went into identifying, understanding, and remediating the incident further supports network defense and response processes across the board. For your own organization at least, and ideally many others. This chapter describes the various tasks you should do in the Exploit phase of F3EAD and will make the process a little less daunting and, dare we say, maybe even a little exciting.

###### Note

We’ve discussed the occasional difficulties between military jargon and common information security vernacular, and Exploit just happens to be one of the big ones. In information security, we tend to almost exclusively use the term exploit to refer to the exploitation of a vulnerability that will give an adversary system access or information. By contrast, in military terms, exploit more broadly means to take advantage of and is used not only in the context of system vulnerabilities but many other contexts as well. In the context of F3EAD, it refers to extracting information that has been collected during the course of pre-operations. When lined up with the traditional intelligence cycle, the Exploit phase can be considered a combination of collection (though usually more of an aggregation of internally gathered information) and processing to get that information into a usable format so that it can be analyzed.

# Tactical Versus Strategic OODA Loops

In Chapter 2 we introduced the OODA loop model and discussed how it applied to incident response. To review, OODA stands for Observe, Orient, Decide, Act, and the general rule of thumb is that the first side to get through the OODA loop will gain a competitive advantage by acting on complete information first. In the first three phases of F3EAD - Find, Fix, and Finish - we moved through an OODA loop aimed at giving incident responders a ***tactically competitive advantage.*** In the second half of F3EAD - Exploit, Analyze, and Disseminate, the goal is to gain a ***strategically competitive advantage***. In short, we want to understand the adversary so well that their usual tactics and techniques will no longer work against us. The adversary will have to take a step back, regroup, try to come up with different ways to carry out their tasks - all things that will take time and make their tasks, and their lives, more difficult. They also give us more time to continue to build defenses and resiliency against them. 

Depending on the structure of your team, which we will discuss in length in chapter 10, you may actually be running these two OODA loops at the same time. The incident response portion of your team may be heads-down fighting in a tactical OODA loop while your intelligence team begins gathering the information they need for their strategic OODA loop. If those functions are one and the same, the tactically-focused OODA loop may take priority and you will complete Find, Fix, and Finish before ever beginning to execute the Exploit phase. Either method is fine, the important part is that you do actually move into the second phase of operations, because a tactical advantage may last for a few days or weeks, but a strategic advantage will have a long-lasting impact and give you the time you need to prepare for future intrusions which undoubtedly will come. 

In a strategic OODA loop, the Exploit phase covers both the Observe and Orient steps. In these steps, you begin to sift through the mountains of data that has been gathered during the incident response operation, gathering or aggregating additional information from previous incidents or external sources related to the incident, and capturing what this information means to your organization in the past, the present, as well as potentially in the future. This can seem like a daunting task, so it is helpful to know where to start. 

# What to Exploit?

When F3EAD is not properly implemented or is not carried out completely, you likely will find yourself dealing with the same intrusions or types of incidents time and time again, and likely not too far down the road. This is not only a waste of time, but may result in additional scrutiny if the intrusion results in the loss of sensitive customer data. In September 2017, the credit reporting company Equifax experienced an intrusion that resulted in unauthorized access to highly sensitive information of 147 million people. In 2019, the US Federal Trade Commission filed a complaint claiming that Equifax failed to properly address a known vulnerability in their web application system, resulting in the breach. This claim set the stage for future companies to have their security practices, including their incident response processes and follow-up, closely evaluated following an incident. Focusing on the Exploit phase will make it much easier to identify additional risks and call out actions needed to completely address an incident.

In the previous Find, Fix, and Finish stages of the cycle, we focused on a specific attack, a specific adversary, and the specific actions needed to deal with that particular incident. If you happen to identify or mitigate an attack by an adversary leveraging similar tactics, it is a happy coincidence but is by no means intentional. By the end of the Finish phase of the ROAD RUNNER intrusion, we had identified a large amount of information around the intrusion, the actors behind it, and how they operated. But even though that information is organized in a way that facilitates the rapid-response nature of incident response, that doesn’t necessarily mean it is in the right format for follow-up intelligence analysis.

In the Exploit phase, we begin the process that ensures that we not only address, but *learn from* the incident. We focus on the threat, and not just the enemy. Because of this, it is important that we not only extract technical indicators related to the particular attack, such as malware samples and command-and-control IP addresses, but also the overarching aspects that led to the intrusion and allowed the attackers to be, at least to some degree, successful. This includes information about the vulnerabilities or weaknesses that were targeted in the attack, and the information or systems that were targeted, and the process weaknesses that may have prevented us from identifying and acting sooner. We are not just trying to protect the network from a replay of the exact same attack, but to understand the various factors such as policies, technical vulnerabilities, or visibility gaps that led to the successful intrusion and to develop protections or detections for them as well. Because of this, we believe that there is very little information that should *not* be exploited and analyzed—but this, of course, makes managing that information complex. 

To make this process a little more manageable, we like to break information down into the following categories:

*   **Technical indicators** - IPs, domains, hashes, certificates, etc. and their contextual information
    
*   **Tactics and techniques** - how the adversary carried out their operations, most often captured using VERIS or ATT&CK - which we will describe in detail later in this chapter. CVEs or CWEs can also be captured in this category.
    
*   **Supporting information** - non-technical or tactical details of the intrusion - who was targeted, what was going on in the company (or in the world) that may have motivated the intrusion. What was the impact? 
    
*   **Links or references** to information related to the incident or the adversary that was shared, was publicly available, or used as a reference in the course of the investigation. 
    
*   **Internal actions taken or request made** - one very important aspect that should not be overlooked is what was done in response to the incident, and what requests for additional action were made for the future. For example, if the incident response report called out that more frequent monitoring of company credentials on underground forums would prevent similar attacks in the future, that recommendation (or possibly commitment!) should not be lost in the aftermath of the incident. 
    

After deciding what information should be exploited, it is necessary to extract that information from incident data, standardize it, and store it for future analysis and reference. This will require a process - ideally one that is documented and can be shared with others so that it can be repeated and improved upon moving forward. Processes will vary from team to team, however there are a few key components that all Exploit-focused processes should have: Gathering information, storing information, and managing Information. 

# Gathering Information

If your organization is anything like most of the ones we have seen, at the end of the investigation you have an “Official Spreadsheet of Doom” (plus five or six non-official versions), thirteen files of notes, several photos of whiteboards, and a slack channel full of hypotheses, links to external documents, and a variety of “let’s follow up on this in the future” comments. The difference between this and some of the best process-driven teams we have seen is that the more advanced teams clean things up periodically along the way - we have yet to see, meet, or even hear about a team who doesn’t have somewhat messy, creative, but effective processes in the middle of an intense incident. If you are staring at a mountain of disorganized data in as many different structures as you have team members, do not fret. The Exploit process will help you get things in order so that you can learn and adapt moving forward. 

Depending on how you manage your incident-response data, it is entirely possible that the most difficult part of the Exploit phase will be finding the important bits of intelligence from the investigation. When it comes to gathering incident-response data, we have seen it all—from elaborate systems, to Excel spreadsheets, to Post-It notes with IP addresses stuck to a whiteboard. There is no wrong way to gather that data, but if you want to be able to extract it so that it can be analyzed and used in the future, there are certainly some ways to make the process easier.

When you are dealing with exploiting information from a previous incident, you are often limited in the data that you have available. One of the goals of intelligence-driven incident response is to ensure that the incident-response process captures the information needed for intelligence analysis, but if you are just beginning the process of integrating operations and intelligence, you may not have been able to influence what information was gathered (yet). A good starting point for the Exploit phase is to understand exactly what you have available. We have found that the information that is currently available usually falls into one of two categories: high-level information, and technical details such as malware analysis.

If you have only high-level information in the form of a narrative about the incident, you will be looking at extracting strategic-level details. If you have access to detailed malware analysis, you can extract tactical-level details about the malware’s functionality. Initially, you may have access to only one level of information or the other, but ideally, as you implement this process in your organization, you will be able to gather both the technical details of an incident as well as the strategic information on the information targeted and what the impact was. Being able to combine information across all the levels is one of the things that makes intelligence most powerful.

## Information Gathering Goals

The first thing to consider when gathering data, no matter what the source, is what are your goals. These come in an interesting variety, but the most common ones are:

Indicators of Compromise
: 

Everyone always wants to say indicators of compromise aren’t useful anymore, but even the best adversaries don’t always change everything. While there may be more durable methods of tracking adversaries you’d be making a mistake not to pull even the IOCs at the bottom of the Pyramid of Pain and adding them to your pile of threat data.

Signatures
: 

While not always available, depending on the product, you may be able to pull out even more advanced compound indicators in the form of signatures. Snort and Yara are most common, and it will often depend on your tech stack, but even product specific signatures can be translated into other tools.

Tactics, Techniques and Procedures
: 

While not quite as atomic, another common approach after the rise of ATT&CK, is to pull out how an adversary operates in the form of TTPs. 

Strategic Data
: 

This is a bit fuzzier, but in a lot of cases, especially reading 3rd party data, it’s the non-technical data that is most interesting. Information about attribution and adversary motivation can be harder to capture but is often very high value.

So where do we pull this data from?

## Mining Previous Incidents

If you are reading this book with the intention of understanding how to start integrating operations and intelligence, it is important to note that you don’t have to start with the next incident; you can go back through previous incidents and exploit that information. In fact, this type of activity is a great way to familiarize people in your organization with the F3EAD process and help them feel comfortable with it as you build your threat profile. Digging through and analyzing previous incident data can give you a great idea of the types of threats facing your network and identify the visibility or information gaps you may need to address as you move forward. Whenever possible (and hopefully it is always possible) include dates and date ranges when mining information from previous incidents.

## Gathering External Information (aka Conducting a Literature Review)

In addition to internal information about an incident we can also gather considerable information about adversaries from external sources. While you are a beautiful and unique organization your adversaries probably aren’t brand new and just focused on you exclusively. Chances are you’re not the first organization that they have targeted, or the first organization to share information about tactics, techniques, or other organizations that were targeted. While it is important to remember that attribution based off of things that “seem similar” is often not useful for creating an exact roadmap of what your organization should do or how you should respond to an incident, it can help provide additional context or situational awareness. It can help to understand what is similar with other known incidents and what is different, where different actions or analysis may need to be made. In this sense, it can be similar to a literature review in academic research. A literature review involves researchers looking at what previous studies, papers, or analyses have been conducted, what conclusions they drew, where they are similar and where they are not similar to your own research. The goal of a literature review is two-fold - to identify the unique aspects of your research that fill in gaps in previous research as well as to build upon and further the overall understanding of a topic. In intelligence-driven incident response we want to build upon our own knowledge, and if sharing is possible, further the collective understanding of threats to help the entire community grow stronger and more resilient to threats. Whatever the scope of your knowledge-furthering, it is important to understanding what has been reported externally about the actors, indicators of compromise, or other information related to your own incident. However, like with a literature review, remember that information found during this process should not be directly mixed with your own data and findings without a clear citation. In many cases, this “literature review” should be kept in a separate section and referred back to during the next step of F3EAD, the analysis phase.

# Extracting and Storing Threat Data

At the end of an investigation—whether immediately following or six months after the fact—you will likely have a plethora of data. Your task in the Exploit phase is to take that information, whatever it may look like, and structure it into a format that can be analyzed and used moving forward. While there are many different ways that an organization may go about this task, we have found that they typically fall into two categories - mostly manual, and platform-based. We will walk through these two different approaches using two specialized tools - the common spreadsheet, and the more exotic graph-based centralized intelligence platform, but first let’s talk about standards. 

## Standards for Storing Threat Data

No discussion of threat intelligence would be complete without a discussion of the various data standards. Many debates have been had - both over Twitter and in hallways around different security conventions - about which standards are the best, which are just fads, and which ones are experiencing “a come back.” These debates aside, the standards you choose are more than anything based on the unique needs of your organization and what you choose may look different than what someone else does. We are about to dive into the details of the various data standards that may make you never want to talk about them again, but please stick with us. Data standards will make your life a lot easier once you find the one that works for you - if it helps, think of yourself as a chef planning out your *mise* *en* *plate,* determining which style of knife cut and which containers are going to make for the easiest meal service. 

## Data Standards and Formats for Indicators

There are several groupings of data standards that are commonly used to store and share technical threat data. No one magical standard rules them all - despite the industry’s best efforts - so the best approach is to understand what standards exist and determine whether one of them will work for you. For example, if you are part of sharing organizations that utilize STIX/TAXII to share data, that may be a good choice for you to store your own data. If your organization has already invested in security tools that use a specific format such as OpenIOC, that may work best. If there is more than one standard that you need to work with (which we see quite often), the next best approach is to make sure that you understand the fundamentals of the various standards and be prepared to do data-field mapping, because you likely will be dealing with information in one of these data formats at some point.

### OASIS Suite - aka STIX/TAXII

STIX was initially created in 2010 as way to standardize the information that was captured about cyber incidents, and to facilitate the automated sharing of information between organizations. It was originally maintained by the MITRE Corporation on behalf of the Department of Homeland Security. The Oasis Cyber Threat Intelligence CT, a component of the Oasis open standards organization, took over MITRE’s role in supporting the CyBox, STIX, and TAXII data formats. These standards are some of the more well-known standards, in part because of US government adoption of this suite. Because of this, many of the Threat Intelligence Platforms have integrated STIX - either STIX1 or STIX2.X, along with TAXII, the transport mechanism for STIX data. 

#### STIX 1

The original version of Structured Threat Information eXpression (STIX 1) was built with building blocks called “CybOX”, aka Cyber Observable eXpression. CybOX is made up of observables, which are *defined objects with stateful and measurable properties.* There are multiple use cases for CybOX, ranging from event management to malware analysis and information sharing. There are a large number of CybOX objects for capturing observables, although not all of them are directly relevant to incident response. <ref [https://stixproject.github.io/getting-started/whitepaper/](https://stixproject.github.io/getting-started/whitepaper/)\>

In STIX v1, CybOX are the basic units, however the “STIX” portion of the standard allows for more contextual detail to be added to the CybOX objects. This enables further analysis and is a huge benefit when sharing threat data. These additional context fields include Threat Actors, Campaigns, Victim Targets, and TTPs. This allows you to take individual observables that are captured through CybOX and chain them together and add more context. This is when threat data can truly begin to become threat intelligence. Although it is good to know that a certain file is malicious, it is more useful from an analytic perspective to know that the file was used in a particular campaign targeting victims in a specific sector, and that after execution of the file, the actors attempted to exfiltrate intellectual property. 

While it is interesting to look back on STIX 1 as a way to see how systems developed, one key component is that STIX 1 wasn’t really meant for people to use it, but instead to be tightly integrated with a specific set of tools. As the need for information exchange and sharing expanded beyond just a small group of similarly-minded organizations, the standard needed to evolve as well. And evolve it did.

#### STIX 2.X

STIX 2 was introduced with several improvements that made its adoption easier for many practitioners. One major change was that STIX 2 used JSON format rather than XML. STIX 2 also reduced the complexity by merging the two data standards (CybOX and STIX) into one standard, called STIX Cyber observables. STIX2 also introduced two top-level Relationship objects, “Relationship” and “Sightings”, which more clearly define the association between different observables. <ref [https://oasis-open.github.io/cti-documentation/stix/compare](https://oasis-open.github.io/cti-documentation/stix/compare)\>

When STIX, or any standard, is used fully, it can be a great tool for analysis, but just remember that work is required to get all of the information captured! STIX is built into many cyber threat intelligence platforms, making it easier to use, however it is still important to have a solid process for determining what gets captured in this format, and making sure that information on insights from an incident is not lost because it never makes it out of the notebook and into the platform.

#### TAXII

TAXII is a transportation and sharing framework and comprises four services: discovery, poll, inbox, and feed management. TAXII is the way that STIX is shared between entities or organizations. TAXII has three primary transport and sharing models:

Subscriber
: 

In this model, one central organization shares information with partner organizations, without the partner organizations sending any information back. This is the model that is most commonly seen with threat-intelligence providers, either commercial or open source, who send information to customers.

Hub and spoke
: 

One organization or source serves as the central authority for information sharing. It pushes information to other organizations, and when those organizations want to share back, they send information to the central organization, which redistributes the information to the group.

Peer to peer
: 

This model can be used by any two or more organizations that want to share information directly without going through a centralized organization. Several mesh networks utilize this model as well.

In most cases TAXII is used as a top down distribution model, such as from an ISAC, which means the Subscriber model. If the others are in use, we haven’t seen them.

### MILE Working Group

In addition to the Oasis Suite, another set of data standards is actively maintained and updated by the Managed Incident Lightweight Exchange (MILE) Working Group, which includes the following:

#### Incident Object Definition and Exchange Format (IODEF)

[RFC 5070](https://www.ietf.org/rfc/rfc5070.txt), first published in 2007 defines IODEF as “a data representation that provides a framework for sharing information commonly exchanged by Computer Security Incident Response Teams (CSIRT) about computer security incidents.” IODEF is an XML-based standard, and is used by groups such as the anti-phishing working group and ArcSite. It includes tags for sensitivity and confidence level. [#iodef\_scanning\_event](https://atlas.oreilly.com/oreillymedia2/intelligence-driven-incident-response/editor/master/07-exploit.html#iodef_scanning_event) shows an example of the IODEF format capturing information on scanning from the original RFC.

#### IODEF scanning event

Real-time Inter-network Defense (RID). Just as STIX has TAXII to facilitate the exchange of information in STIX format, IODEF and IODEF-SCI have RID. The goal of RID is to allow different organizations with incident data to share that information in a secure, easy-to-manage fashion. RID is defined in [RFC 6545](https://tools.ietf.org/html/rfc6545), and RID over HTTPS is defined in [RFC 6546](https://tools.ietf.org/html/rfc6546). Similar to TAXII, RID has several options for information-exchange models, including direct peer-to-peer and mesh peer-to-peer and client-to-subscriber.

#### IODEF—Structured Cybersecurity Information (IODEF-SCI)

This extension to IODEF provides a framework for capturing additional context around incident data. [RFC 7203 ](https://tools.ietf.org/html/rfc7203)defines the standards for IODEF-SCI and was first published in 2014. IODEF-SCI provides a structure for embedded additional contextual information into IODEF documents, including Mitre’s Common Attack Pattern Enumeration and Classification (CAPEC), Common Vulnerabilities and Exposures (CVE), Common Vulnerabilities Scoring System (CVSS), and several other standards.

### OpenIOC

As we discussed previously, the term *IOC*, or indicator of compromise, was popularized by Mandiant. In addition to coining the term, Mandiant developed a standard for capturing IOCs, which is called OpenIOC. *OpenIOC* is an XML-based schema (you may have picked up on a trend here) that is designed to capture and categorize forensic artifacts from compromised hosts as well network indicators related to malicious communications or other malicious activity. Mandiant identified over 500 possible artifacts that can be documented using OpenIOC. However, the framework also allows for customization and the creation of new fields as needed by organizations using OpenIOC. OpenIOC is interoperable with STIX, and documentation has been published on [how to transfer between the two standards.](https://github.com/STIXProject/openioc-to-stix)

You will likely find yourself needing to convert data from one standard to another as you share and receive both threat data and intelligence. It is important to be aware of the various fields and components of the standards, because as you move between them (for example, from STIX to OpenIOC), you could lose or gain certain data fields. If you are unaware of the differences between the standards, you may find yourself searching for something that you *know* you had captured, but did not make it across in the conversion. When converting from one data standard to another, be sure to identify the important fields within the current standard, and then identify the equivalent field in the standard to which it will be converted.

While OpenIOC was an ambitious program is has largely been deprecated at this point, as Mandiant merged with FireEye, combined tech stacks, and broke them up again as the companies separated. Beyond Mandiant, OpenIOC never really gained traction enough to become an industry standard, and eventually lost out to STIX2.

## Data Standards and Formats for Strategic Information

As we mentioned earlier, indicators that can be captured using the preceding formats are only half of the picture that you want to capture. Indicators are extremely useful for detection and response, but it is also important to gather other contextual information that will support strategic analysis. Although that information can be stored by using formats such as STIX, standards for capturing technical information are not often the best fit. Often such information ends up being stored in documents or Powerpoint slides, if it is captured at all. Not as many options exist for storing strategic information as for storing technical information, but frameworks can be utilized to avoid losing this critical component of incident information. We will explore three of the primary standards for storing strategic information: ATT&CK, VERIS, and CAPEC.

### ATT&CK 

We have mentioned ATT&CK several times already in this book because it truly has taken the threat intelligence community by storm over the past few years. As the cry was made to move on from just IOCs to actor tactics (and thus up the Pyramid of Pain) and techniques became louder and louder starting around 2018, Mitre’s ATT&CK was ready to answer the call. 

Mitre’s Adversary Tactics, Techniques, and Common Knowledge (ATT&CK) was created in 2013 with the goal of categorizing attacker behaviors throughout the attack lifecycle and was originally focused on attacks against Microsoft Windows. It focused on tactics - which were described as short-term, tactical adversary goals of an attack, and techniques - which were the means through which adversaries achieved their tactical goals.(ref to [https://www.mitre.org/sites/default/files/publications/pr-18-0944-11-mitre-attack-design-and-philosophy.pdf](https://www.mitre.org/sites/default/files/publications/pr-18-0944-11-mitre-attack-design-and-philosophy.pdf)) Since then it has grown to include attacks against Linux and MacOS, along with additional matrices describing pre-exploitation activity (PRE-ATT&CK), mobile platforms (Mobile ATT&CK), as well as Industrial Control Systems, Cloud Systems, even Containers like Docker and Kubernetes. 

The ATT&CK object structure includes tags, fields, and relationships. Tags are data points that can be used to filter or pivot off of; fields are used for free text to describe different tactics or techniques, and relationships are fields that define the relationship between different entities. 

The model and its relationships can be seen in the image below ([Figure 4-1](#ch04.html#att_ck_model_relationships)). Each entity, Adversary Group, Techniques, Tactics, and Software all have their own fields that can be used to capture additional relevant information, making ATT&CK a model that can be used to capture both tactical and strategic information.

![](assets/exploit_403850_01.png)

### VERIS

The Vocabulary for Event Recording and Incident Sharing (VERIS) is a JSON-based standard that is well-known for supporting the Verizon Data Breach Incident Report (DBIR). The VERIS framework captures information that falls into four categories, known as the Four A’s: Actor, Asset, Action, and Attribute, all of which answer a question about the incident.

Actor
: 

The Actor field answers the question, “Whose actions affected this asset?” This field captures high-level information about the actors responsible for an incident. The data schema enumerations for Actor include whether the actor is an internal, external, or partner actor, as well as the actor’s motivation.

Action
: 

The Action field answers the question, “What actions affected the asset?” Action includes things such as how the attackers were able to get access, including the use of malware, hacking, or social engineering. It also includes the specific vector, such as exploiting a known vulnerability, or using phishing emails.

Asset
: 

This field answers the question, “Which assets were affected?”—an incredibly important question to answer from a strategic perspective. The enumerations include information about the type of asset affected, as well as information about its accessibility and management.

Attribute
: 

The Attribute field answers the question, “How was the asset affected?” It uses the traditional Confidentiality, Integrity, Availability triad.

VERIS also captures information about the timeline and impact of an incident. These fields provide places to capture how long it took to identify, contain, and remediate an incident and how severe the impact is for the organization affected.

The primary use case for VERIS is not generating rules or alerts, but for helping organizations understand the risks that they face. Therefore, the information is not as detailed nor as technical as the information that is captured in STIX or the formats that we mentioned previously. However, it can be used to tell a more complete story of what happened in a particular incident.

### CAPEC

The Common Attack Pattern Enumeration and Classification (CAPEC) framework was originally designed to help with the development of secure software. The concept behind CAPEC is that if software developers were able to understand the common ways in which attackers targeted and exploited software, they would be able to design and build software that is not susceptible to those attacks. Rather than just capturing specific technical details, CAPEC attempts to capture the entirety of an attack as an attack pattern, which includes information on attack prerequisites, related weaknesses, related vulnerabilities, and attacker steps.

An organization can learn a great deal from an attack when a clear picture of what happened is available and is captured in CAPEC. An analysis of attack patterns over time can provide an understanding of the way attackers operate, the way they adapt to security measures, and any additional measures required to protect an organization.

## Process for Extracting

Once you have identified the format you want (or need) to capture data in and where you will store it, it is important to develop a process for extracting all the relevant information. This is the part of the Exploit phase where the rubber meets the road. While we wish we could spend this section outlining the exact step-by-step process that you should use, the truth is that like everything else in cyber threat intelligence, there are unique components to every CTI program that will make these processes vary case by case. We are instead going to outline the process for developing a process, with the goal of helping you outline something that will work for you long term and that you can easily modify if your needs change. 

### Step 1: Identify your goals

Be as specific as you can be here, don’t just say “our goal is to manage the data from the incident” although it can be a good start. A clear goal would include the intended outcome of the exploit phase, who it will be supporting, and the intended impact. *“Capture all information - including IOCs, TTPs, target information, and strategic information in a spreadsheet with an executive summary of the incident so our current team of 5 intelligence analysts can further analyze and make additional recommendations for detections and mitigations.”* Written out like this, a goal becomes a roadmap for the process that you will follow. 

### Step 2: Identify your tools 

We use the term “tools“ here very lightly. A tool is anything that is going to help you achieve the goals we outline in step one. Some organizations have threat intelligence platforms (or TIPs, covered later in this chapter), which may have built-in functionality like automated indicator extraction and categorization. If you don’t have a TIP there are still many other resources that can be used to help, including scripts, spreadsheets, non-TIP databases, and productivity and collaboration tools. At this stage you should also call out whether or not you have the right tools to achieve the goal you laid out - for example, if the goal involves sharing information with a team of five analysts, you will need some sort of collaboration tools. If you do not have any tools that will facilitate sharing information, then you will have a very difficult time meeting that goal. 

### Step 3: Identify the system or process you will use

This can be the most diverse step out of these three because there are so many different ways that an analyst can start to get data into the tools needed to achieve the Exploit goals identified in step 1. Approaching this undertaking in a systematic, ideally documented fashion is the best way to make sure that it is done completely and there isn’t information missed or overlooked in the process. One process may involve the following tasks:

*   Identify the location of all data sources that will be aggregated as part of the Exploit process. 
    
*   Create a checklist of all of the sources to keep track of what has been exploited.
    
*   Based on your tools, complete the necessary logistical steps to prep them for exploitation - whether this means creating a new file based off of a template, identifying that tags that will be used when entering data into a database, or beginning a new wiki page that will house all of the information from this incident. 
    
*   Working down the checklist you created of data to be aggregated, begin moving data from disparate sources into your tool of choice, checking off one source once it is complete.
    

Having a clear process to get from a pile of data post-incidents to a structured, comprehensive set of information ready for analysis is one of the best ways to set yourself up for success. The system you develop may involve multiple people focusing on different functions within the system, or it may just be a process for yourself. You may add timelines or milestones into the process to keep you on track and accountable, or you may find that every incident is so different that timelines don’t matter as much as just moving through the steps. The important part is to document how you will move through the data extraction process and to be comfortable updating or revising the process as goals, tools, or needs change. 

### Step 4: Launch and iterate  

This where you dive in. This can be the hardest part of the whole process - even if you have the right tools and a solid process you have to actually start working through the steps in a systematic way. Try things out to see what is working and where you are running into hurdles. Don’t be afraid to change your process up, start identifying new tools you may need, or new skills that would make the process easier. If you find a method or approach that is especially useful, make sure to document it for future analysts (or for future you) 

# Managing Information

Managing information does not simply involve capturing the individual indicators of compromise or artifacts from an investigation. A great deal of additional information needs to be captured as well so that you know how to manage and handle all the types of information moving forward.

Some key things are critical to capture in order to help manage information:

Date:
: 

When was this piece of data or information seen? This will help with analysis as well as support expiration or retirement of data, essentially determining when data is still good to act on and include in analysis and when that data is no longer valid.

Source: 
: 

Few things are more frustrating than not knowing when and where a piece of information came from. Capturing the source of the information will help if you ever need to go back to get more information or if you want to assign the confidence of that piece of information, both of which will be useful during the analysis phase.

Data-handling information: 
: 

Often data will need to be handled differently based on the sensitivity and the source. We recommend using DHS’s Traffic Light Protocol (TLP), which dictates how information can be shared. The TLP standard was updated in August 2022, additional information can be found [here](https://www.first.org/tlp/).

TLP:CLEAR:
: Publicly available information that can be shared with anyone using any method.  

TLP:GREEN:
: Information that can be shared with peers and partners, but not using publicly available channels, such as posting it on a blog or tweeting it to journalists. Seriously, don’t do that. 

TLP:AMBER:
: This information can be shared with people within your organization and its clients, but not outside it, and not using public channels. TLP v2 helped to clear up some confusion about the use of TLP:AMBER by specifically including client of an organization, and introducing the new caveat TLP:AMBER+STRICT, which means that the information must stay with the organization with whom it is shared, and cannot be shared with external parties, including clients. TLP:AMBER and TLP:AMBER+STRICT information should not be shared using public channels. 

TLP:RED:
: This is extremely sensitive information, usually related to an ongoing incident or investigation. It should not be shared outside the specific recipient, not even within the recipient’s organization, without prior approval. Often TLP:RED information will be reclassified as Amber or Green once the situation has been resolved.  

Duplicate data:
: 

It is important to make sure that you are not duplicating data by accidentally capturing the same incident data or threat report more than one time. However, at times you will receive the same information from more than one source, and when that happens, it is important to capture that. Receiving the same indicators from multiple places, such as from an internal investigation *and* from a threat report from the FBI can have serious implications, but if you are not able to capture the details around both sources, you can seriously hinder the analytic process, which you will be moving into next.

Keeping these things in mind as you begin to store and manage your data will make using and maintaining your data much easier.

## Threat-Intelligence Platforms

As you can probably tell from our coverage of standards and the numerous requirements for managing all the information that you have exploited during an investigation, that capturing and analyzing all of this information is no trivial task. A threat intelligence platform is often used to simplify that process and make gathering, storing, and searching this information easier.

A threat-intelligence platform (TIP) is a database and user interface specifically designed to handle threat information. Various types of threat intelligence platforms exist—some that specialize in information sharing, and some that focus on the storage and management of large quantities of IOCs. Most TIPs can ingest information in the tactical formats described earlier in the chapter, and capture the additional information needed to manage the information as well. Using a TIP will significantly reduce the amount of work that needs to be done in the Exploit phase of F3EAD. Several popular open source platforms are available, and a variety of commercial solutions as well. We will discuss those options next.

### MISP

The Malware Information Sharing Platform (MISP) is a free option for managing malware-based threat data. MISP was created by a group of developers in conjunction with NATO’s Computer Incident Response Capability (NCIRC). MISP includes a database with a user interface that enables organizations to store both technical and nontechnical information related to attacks in order to facilitate correlation and sharing of information on threats. MISP can export information in OpenIOC, plain text, CSV, MISP XML, and JSON formats so that it can be used to support intrusion detection and prevention. MISP also has a robust sharing capability enabling users to share with other MISP users or groups. You can get more information on MISP [on GitHub](https://github.com/MISP/MISP).

### CRITs

Collaborative Research into Threats (CRITS) is another open source tool for managing and sharing threat data. CRITs was developed by MITRE, and therefore was designed to work with STIX and TAXII. CRITs stores threat information and includes the ability to add confidence and severity to the indicators that have been captured. It integrates with TAXII services to facilitate sharing, and is therefore a good choice for many organizations that receive or exchange information with the government or other organizations using STIX/TAXII. CRITs can export data into CSV, STIX, and JSON formats. Information and documentation for installing and using CRITs can be found [on GitHub](https://github.com/crits/crits).

### YETI

Your Everyday Threat Intelligence (YETI) platform is a newer threat intelligence management tool that was released for public use in March of 2017. YETI was designed to enable analysts to organize and analyze the various components of threat intelligence in one place. It supports observables, indicators of compromise, TTPs, as well as general knowledge on threats. One of the great aspects of YETI is that in addition to storing information that you have already found about threats, it can also perform some indicator enrichment, including domain resolution and WHOIS lookups, as well as any additional integrations you want to configure. YETI can ingest data from MISP instances, JSON feeds, XML feeds, and various malware sandboxes. YETI was specifically designed to support many challenges that threat-intelligence analysts have identified in recent years, and to be flexible, as many analysts often need the same information, but have different processes or workflows. YETI has a [GitHub repository](https://github.com/yeti-platform/yeti) where you can find more information on installation as well as documentation.

### Commercial solutions

A variety of commercial solutions are available for threat-intelligence management as well. Most of the commercial solutions include similar features to MISP, CRITs, and YETI, but also manage system configuration, take responsibility for setup and hardware management, and offer support for troubleshooting or feature requests. Commercial solutions can be ideal for organizations with limited development resources that want something easy to set up and maintain.

All of the threat intelligence platforms, whether open source or commercial, have many of the same features and functionality, but may have been designed with a particular use case in mind, whether that is malware-based threat information, a specific information sharing, or supporting and enabling analytic processes. One of the best things about starting with open source threat-intelligence platforms is that you can find the best fit for your organization. If installation and support of the tools are a problem for your organization, you can explore several commercial solutions once you identify the best overall type of platform for your organization. 

# Conclusion

Information that is gained from an investigation should not be forgotten after the engagement is over—whether that investigation occurred at your own organization or happened somewhere else and you are fortunate enough to have access to the data. That information needs to be analyzed and disseminated so that organizations can learn and adapt to threats. Analysis and dissemination are not possible, however, without first going through the critical exploitation phase where information is gathered, processed into a usable format, and stored for analysis. As you learned in this chapter, you have many options for processing and storing this information, from the format it is stored in to the actual database it is kept in and the interface used to access it, so take time to explore your options and find a system or combinations of systems that will work well for you. Once the Exploit phase is completed, it will be much easier to move into the next phase of the F3EAD cycle: Analyze.

# Chapter 5. Analyze

> “If you do not know how to ask the right question, you will discover nothing.”
> 
> W. Edward Deming

All of the information that you have gathered has been exploited and is now sitting—formatted and standardized—in a database or a threat-intelligence platform. So now what? The information sitting there does little good until it is analyzed. The Analyze phase of F3EAD is one of the hardest to articulate but also one of the most important. This is the phase where we take data and information and process it into intelligence. This chapter covers the basic principles of analysis, models such as target-centric and structured analysis, and processes to assign confidence levels and address cognitive biases.

# The Fundamentals of Analysis

To properly analyze the information you have, you must go through another (thankfully, smaller) version of the intelligence cycle. You must decide what your requirements are, or in other words, what questions you are going to answer. You need to collect the information that you will use to answer those questions. Most of that collection will come from the information you gathered throughout the investigation and standardized in the Exploit phase, but other information will be needed to enrich or augment that information so it can be properly analyzed. Therefore, it may be necessary to continue to collect data as you move into the Analyze phase. The Analyze phase of F3EAD captures the entire intelligence cycle, pictured [Figure 5-1](#ch05.html#fig_1_analysis_within_f3ead).

![](assets/analyze_837710_01.png)

During the response to the Road Runner intrusion, we identified domains and IP addresses that were used in the exploitation and command-and-control phases of their operations. That information helped us in the Fix and Finish stages, and will continue to help us as we analyze the intrusion, but in a different way. Rather than only identifying the technical details of the attack in order to respond and remediate, we can analyze those same domains and IPs to identify patterns that can be used to better understand the attacker’s tactics. This process involves gathering additional information about the domains and IPs, including who they were registered to, and how the attacker used them in order to determine whether patterns can be used to identify or predict future behaviors. This new information is then analyzed, intelligence gaps (critical pieces of information that are needed to conduct analysis) are identified, and more information is gathered as needed.

## Dual Process thinking

You may be picking up on the fact that thing have slowed down a bit by this phase of the F3EAD cycle. That doesn’t mean that these subsequent steps are any less important or significant. What it actually means is that the mental processes required here - especially in the Analyze phase - require both significant cognitive efforts and significant cognitive rigor that cannot be accomplished by using the “fast thinking” often employed in incident response. Instead, we need “slow thinking” - though probably still on a deadline (often even short deadlines). 

The concept of thinking fast and slow was popularized by Daniel Kahneman in the book by the same name (Thinking, Fast and Slow) however the theory of the dual process of thought have been studied for decades. The general idea states that there are two modes of thinking: “fast thinking”, also known as *System 1 thinking*, is a quick, automatic process often based on association and experience, relying heavily on pre-existing mental models. This type of thinking can get us out of a jam when time is short and action is needed quickly. When using this system, the mind makes an assumption that the risk associated with minor flaws in intuitive judgement are less significant than the risk of not acting fast enough. The flaws associated with *system 1 thinking* include cognitive biases and misapplied mental models, which may not be that serious when outrunning a tiger, but could cause a lot of problems when making decisions that will impact you long term. The second system, slow thinking or *system 2 thinking*, is a deliberate process of reasoning that aims to counter the impulsive biases that are so effective with *system 1*. With *system 2 thinking*, not only do you need to take the time to counter things like biases, but you also need to make sure that you have a firm grasp on the context of the different pieces of information that you are analyzing and are thinking through second and third order effects. Understanding complex networks and situations takes time, and without the time to think things through it is very likely that you will come to at least some faulty judgements. 

In this chapter, we are focusing on the *System 2* process of thinking as a way to help generate greater insights that can be clearly articulated, defended, and shared with others who will be able to follow the reasoning and come to more or less the same analytic conclusion. This is not an easy task, however throughout the chapter we will be presenting different approaches, methods, and frameworks that can be used to support this analysis. We will also cover some additional mental models that can put analysts in the right frame of mind for *thinking about thinking,* or metacognition, a concept that has been discussed and debated since at least the time of Greek philosopher Aristotle. 

## Inductive, Deductive, and Abductive Reasoning

While thinking about approaching a problem we don’t usually think about how we think about problems. It’s a little bit like driving a car to work, it’s easy to just pick a “normal” way to go and head the same way no matter what. But it’s not always that easy, is it? Sometimes there’s traffic, sometimes there’s construction, sometimes you need to stop by the store, sometimes you just want a different view. In these cases, you may take a different route to get to the same place. Reasoning can go the same way. 

Before we start, it’s important to know reasoning, like so many things we do, follows the typical intelligence axiom when you ask which type of reasoning is the best: it depends. Like your trips in the car, which type of reasoning you use depends on what you’re trying to accomplish, which is fine. The issue is when you don’t know why you chose the approach you have. The fact is, in a lot of cases analysts do the same thing, without really thinking about why (kind of like when you drive somewhere on mental autopilot). In a lot of cases, analysts (both intelligence and security) do the same kind of reasoning the same way. You may even do it yourself. So, take a step back, and let’s think about how we solve problems and why we should reconsider different approaches. 

### Deductive Reasoning

> Deduction: the deriving of a conclusion by reasoning specifically: [inference](https://www.merriam-webster.com/dictionary/inference) in which the conclusion about particulars follows necessarily from general or universal premises
> 
> [Merriam Webster, Definition 2a.](https://www.merriam-webster.com/dictionary/deduction)

Made famous by everyone’s favorite detective, Sherlock Holmes, deduction isn’t usually what people think it is. In short, a deduction is always based on universally accepted premises. Another way to think about it is starting with an idea, making an observation or series of observations on causes, and then making a conclusion about the expected effect. 

Deductions are often very direct and simple, not what we think of in investigations. For example, think back to high school geometry. One rule of geometry is that the internal angles of all triangles add up to 180 degrees. If you have a triangle with two 45 degree angles then you can deduce, based on the 180 degree rule, that the 3rd angle must be 90 degrees (you could go one step further given another rule that any triangle with a 90 degree angle is a right triangle and say this must be a right triangle as well). 

While the Baker Street resident sleuth likes to reference it a lot, most detectives aren’t deducing their cases. In most examples of deduction there are both simple premises and nearly perfect information. Deductions are often useful for understanding basic life but not for investigations. We rarely have enough perfect information and can rarely make universal rules.

### Inductive Reasoning

> Induction: inference of a generalized conclusion from particular instances
> 
> [Merriam Webster, Definition 2a.](https://www.merriam-webster.com/dictionary/induction)

The first, and most commonly seen reasoning method in most investigations is induction. Induction is a circumstance where you start with a pile of data, maybe a singular example, maybe a few different examples, and work out a rule that fits all the data. You could simplify this down to induction being the process of using a specific set of cause and effect relationships to build a generalized rule. 

Inductive reasoning is often a shortcut. For example, say you have a new coworker who has a peanut butter and jelly sandwich for their lunch on their first Monday. They have the same peanut butter and jelly sandwich on Tuesday, Wednesday, and Thursday. So, what about Friday? Well, inductive reasoning may lead you to say “obviously, they eat peanut butter and jelly sandwiches every day, so clearly they’ll eat a peanut butter and jelly sandwich on Friday as well”, and maybe they do, but your new coworker asks others if they want to go out to lunch for Friday. 

Inductive reasoning always benefits from more information as it makes it possible to further refine the theories you’re matching against. These inductive theories are only as good as the data available, but this imperfect data also makes it possible to come up with generalizations that aren’t 100% correct, such as assuming our coworker would follow four days of one lunch with a fifth. After the first week, we might refine our theory to assume they always eat lunch out on Fridays, but after a second week, that could change again if you find out they only went out that first Friday to meet coworkers. So, you might need to refine it again. This often manifests as “correlation doesn’t equal causation”. 

The more data, the better the theories will be, but this is a challenge in security investigations. It’s impossible to know how much data we have, whether we can see 10% or 100%. It’s impossible to know how good our theories are, and dangerous to assume they’re too good. We see this happen a lot using link analysis tools such as Maltego. People will often throw data on a force directed graph and start making inductive theories. This can often be something as simple as looking at network flows in a graph and assuming the node with the most connections must be the most important. Is it? Well... sometimes it is, but ask any malware analyst how many pieces of malware reach out to [google.com](http://google.com) or [microsoft.com](http://microsoft.com) to verify internet connectivity? Does that mean those are bad websites? Clearly not. 

### Abductive Reasoning

> Abduction: a syllogism in which the major premise is evident but the minor premise and therefore the conclusion only probable.
> 
> [Merriam Webster, ‘Deduction’ vs. ‘Induction’ vs. ‘Abduction’](https://www.merriam-webster.com/words-at-play/deduction-vs-induction-vs-abduction)

At this point, you might have guessed that abductive reasoning is what we end up using most often in security investigations.  Abductive reasoning is in some ways a combination of the other two (and mostly what Sherlock Holmes is actually referring to). Abduction is the process of taking information that’s available, applying past rules and facts, and using that to come up with a plausible set of causes. This differs from induction where we’re trying to create these basic rules, rather than apply them, and differs from deduction, where you’re taking a set of cases and rules and using them to predict an effect. 

Abduction is always the best way to think about our investigations because unlike both induction and deduction it takes into account that we rarely have perfect information and can rarely come up with universal rules based on the information we do have. In addition, our usual goal in investigation is to discover the cause. For example, if you said “Our website is down with too much traffic” (An effect) and “DDoS for ransom is a common reason for too much traffic” (A rule) then you could come up with a cause “We’re being DDoS’d for a ransom”. 

### What’s the Reasoning for talking about Reasoning?

Many of our most common issues and challenges within cyber security come down to bad reasoning about problems and bad fundamental assumptions. One example is attribution of malware to specific threat actors. For many years the WINNTI malware family was commonly treated as being used by only a single threat actor, and the threat actors and malware were used interchangeably, in this case a bad rule. This itself is an example of induction gone wrong. As a result, this bad rule was applied many times and led to confusion around who was really attacking an organization, as the adversaries’ actions didn’t match the original WINNTI threat actor, and the rule led to confusion. If you look at the MITRE ATT&CK WINNTI Group the confusion is clear as they call out overlaps with Axiom, APT17, and Ke3chang.

In short, if you don’t know how you’re approaching a problem and why you’re approaching it that way, it’s easy to introduce mistakes that could lead to confusion or even failed analysis farther down the line. The first analyst who saw the WINNTI malware and associated it with a single actor may not have been wrong, but the way that became accepted became a problem for many organizations later on. The Key Assumption Check structured analytic technique is meant to help you challenge and avoid these kinds of mistakes, but the best thing possible is to consider how you’re framing your thoughts to begin with and avoid them before it becomes a problem!

# Case Study: The OPM Breach

One of the most significant breaches that is likely still impacting countless people and entities today was the [breach of the United State’s Office of Personnel Management (OPM)](http://bit.ly/2uI6oD1), which resulted in the loss of personal, highly sensitive information about more than 20 million individuals who had undergone a background investigation for a security clearance. In addition to the size and sensitivity of the information that was stolen, the OPM breach is notable because of the multiple, missed opportunities for the attack to be identified and prevented. The intrusion was a complex campaign that spanned years and included the theft of IT manuals and network maps, the compromise of two contractors with access to OPM’s networks, as well as OPM directly. Even when the individual intrusions were identified, no one connected the dots to identify that a larger threat needed to be addressed.

Even before the attackers began their campaign, opportunities existed to take what we know about attackers and how they operate and to identify that a government agency with access to highly sensitive, highly actionable personal information on every American with a security clearance was a high-value target for nation-state attackers.

The complete timeline of the breach is a lesson in how analysis, if done properly and in a timely fashion, can prevent or reduce the devastating impact of a successful campaign. It also shows just how bad it can be when incident responders, managers, and executives fail to connect the dots and see the bigger picture. The OPM breach was a classic and disheartening example of a failure to analyze, even when all of the information was available.

# Analytic Processes and Methods

In incident response, experience with networking, intrusions, and other cyber security fundamentals are key to making quick and accurate decisions, recognizing patterns of malicious activity, and identifying how to mitigate them. As we learned in the chapters on Find, Fix, and Finish, using models such as the kill chain and the diamond model, can help us organize both our thoughts and the data surrounding an incident. With analysis, we rely on a separate set of skills that tie into the knowledge and experience around network intrusions and cybersecurity, but are unique to intelligence analysis. These can be thought of as cognitive skills that underpin any particular area of content-based expertise. Cognitive skills include memory, cognitive processing, logic and reasoning, attention, and cognitive flexibility. These skills - which are helpful in all aspects of life - are also supported by a set of processes and frameworks that can help apply them to a given subject. In intelligence-drive incident response we use Structured Analytic Techniques and Analytic Frameworks such as Target-centric analysis and Victim-centric analysis to help guide our processes (more on these below). 

The processes and methods described in this section are common approaches to analysis and can be used on their own or combined. In intelligence analysis, it is not experience with a particular subject that will make the most difference, but experience with the overall process of conducting intelligence analysis. 

## Structured Analytic Techniques

One of the best primers on Structured Analytic Techniques (SATs) is the book Structured Analytic Techniques for Intelligence Analysis by Randolph Pherson and Richards Heuer Jr, currently on its third edition. While SATs are relevant for all types of intelligence analysis, there are some techniques that are more applicable to intelligence-driven incident response, so we will focus on those. 

The goal of using Structured Analytic Techniques is to give analysts a (more or less) step-by-step approach that is repeatable, explainable to others participating in the analytic process, and can be reproduced or the steps retraced by others. SATs are specifically designed to counter the type of “system 1” flaw we called out at the beginning of the chapter; cognitive biases and misapplied mental models. Structured Analytic Techniques for Intelligence Analysis breaks SATs into six families: 

Getting Organized
: 

Techniques in this family help an analyst at the beginning of the analytic process, when they are staring at a pile of data without a solid idea of where to begin. It includes methods such as sorting, ranking, and checklists. Fortunately, with intelligence-driven incident response you are not starting from a blank slate - you have been gathering, sorting, and organizing data throughout the incident response process and are often not starting from zero. If, however, you decide to spend some time going through old incident data that had not been fully analyzed, it could be useful to begin with methods from this family. 

Exploration Techniques
: 

Techniques in this family help an analyst come up with new approaches, rethink previously held beliefs, or begin to push the boundaries of the biases that tell us that something *isn’t possible.* Exploration techniques focus on methods such as brainstorming and creating mind maps, often focusing on the relationships between different elements. 

Diagnostic Techniques
: 

This family of techniques is used frequently in intelligence-drive incident response - in fact we deep dive into one of its most commonly-leverages methods, Analysis of Competing Hypotheses, in the section below. Diagnostic techniques most closely mirror the scientific method that many of us learned when we were younger, with the caveat that in intelligence analysis we almost never have the complete picture and it’s typically not possible to fully test any hypothesis that is generated. Other methods in this family are Multiple-Hypothesis Generation, Diagnostic Reasoning, and Deception Detection. 

Reframing Techniques
: 

These techniques are a key staple in any analyst’s tool belt, they are specifically designed to help identify when biases or misapplied mental models could be impacting analysis. Many of the “contrarian techniques” covered later in the chapter, including Red Hat Analysis and What If? Analysis are considered reframing techniques. These techniques are best done with a group to better identify flaws in mental models that an individual may have, however they can be done even by individual analysts, such as through a Structured Self-Critique exercise. 

Foresight Techniques
: 

“Predictive Intelligence” is something that every security manager and CISO wishes that their teams could generate regularly. While it is unfortunately not possible to perfectly predict the future, there is a set of SATs designed to help identify driving forces that may indicate a future outcome and be prepared to monitor those to anticipate changes. One method that is particularly useful in intelligence-drive incident response is Indicator Generation, Validation, and Evaluation, which we will cover in the next section. 

Decision Support Techniques
: 

There are times - many times in fact - where an analyst is not conducting analysis to help determine their own next steps, but to know what to present to a senior leader or decision maker to help *them* make a decision. And fortunately, there is a family of techniques that help think through information and structure it in a way to support decision making. These techniques include heavy-hitters also used in business intelligence, such as SWOT Analysis and Decision Trees. 

Here are a few steps that will guide you to set yourself up for productive analysis by helping you decide which SAT to use and identifying who else should be part of the process.

1.  Determine what question you are going to answer, ideally using specific requirements from your leadership. You can conduct multiple iterations of structured analysis in order to answer multiple questions present through requirements, but just as you do not try to test multiple variables within a single experiment, it is best not to try to answer several questions with the same analysis. Even if the processes are only slightly different, it is ideal to keep them separate to avoid clouding your judgment or contaminating your conclusions. If time constraints are present, focus on the most pressing question/requirement and begin to work on that.
    
2.  Identify the nature of the question you are trying to answer through analysis.  Is it future looking? Perhaps you should review techniques from the foresight Family. Are you attempting to describe how a recent intrusion fits into the overall pattern of threats that your organization has dealt with? A diagnostic technique may be more beneficial. Capturing not only the question, but the *nature* of the question will help you on the right path. 
    
3.  Review your exploited data to determine whether or not you have all the information that you need to generate a hypothesis or several hypotheses to answer your question. This involves going through the information you gathered from the investigation, and may require collecting additional enrichment information, which we will discuss later in the chapter.
    
4.  As we mentioned previously, structured analysis can be done as an individual analyst, but many of the techniques are most effective when done as part of a team, so think about who your team should be here. If you already work as part of a team of dynamic analysts then this may be easy - and you are very fortunate! If that is not the case, there are still others you can include in the analysis. Key incident response personnel, malware analysts, red teamers - all of these groups are used to thinking analytically, and one of the benefits of using structured analytic techniques is that it provides a step-by-step process that allows you to leverage unique experiences and perspectives and also address biases that may come with that experience. 
    

In addition to the families of techniques, which can make it easier to narrow down the list of SATs that could be helpful in a given situation, Heuer Jr. and Pherson designated several techniques as fitting into many different families. These are considered *core techniques*, and are often useful with intelligence-driven incident response. The core techniques we will cover in the next section include Key Assumptions Check, Analysis of Competing Hypotheses, and Indicator Generation, Validation, and Evaluation.  We will also cover some very helpful Contrarian techniques to help address biases, which as we will learn, can seriously hinder analysis. 

### Key Assumptions Check

Key Assumptions Check is one of the most leveraged structured analytic techniques, and for good reason. As we discussed previously, our brains look for shortcuts wherever they can. One place they often look to is assumptions that often become the basis of our analysis, whether they should or not. And to be fair, sometimes they *should,* but it is important that we are intentional about the fundamental beliefs we have about our situation. For example, with our Road Runner intrusion, we made an assumption that because we were passed information about a threat from another campaign, when we found some of the same indicators in our network, that it was *because* we were a campaign. While this is a reasonable assumption and it certainly seemed to help us during the Find, Fix, and Finish phases, that does not mean we should never question that assumption. A Key Assumptions Check supports that questioning process. 

A Key Assumptions Check is a systematic approach to identifying the assumptions that are being used to fill in key information gaps and determining whether these assumptions are valid and should remain part of the analytic process. Ideally this is done at the beginning of an analytic undertaking because, as we will see as we move through other common structured analytic techniques, many of them depend on having a firm grasp of assumptions and preconceptions that are part of the analysis. 

Heuer Jr. and Pherson outline a process for conducting a Key Assumptions Check:

1.  Gather the group who will participate in the key assumptions check. This should include those who are familiar with the project or topic, along with some respected analysts who are not intimately familiar with the issue and can provide a fresh set of eyes. 
    
2.  Begin the session by identifying the specific topic to be discussed, and ask each participant to silently write down the assumptions that they have about the topic. For Road Runner, this could include things such as “election campaigns are targeted by sophisticated actors”, and “the information passed to us from the sharing group was a legitimate intrusion and not a false flag or misinformation operation”. 
    
3.  Collect each participant’s responses and write them on a whiteboard, or if conducting the session virtually, on a collaboration document or via screen sharing. 
    
4.  As a group, review the assumptions that have been identified and start brainstorming additional assumptions. One good question to ask to get this part moving is *“what assumptions are so basic and fundamental that we would never even think to question them?”*
    
5.  Once you have a solid list, go through each assumption and ask key questions about it such as “Why do we think this is true?” “How much evidence supports that this assumption is true?” “If this was true in the past, why do we think it is still true?” It is also helpful to propose counter assumptions - not necessarily the exact opposite of the assumption, but an alternate assumption that would significantly change the perception of a situation. 
    
6.  For each assumption, evaluate as a group whether it is a sound or solid assumption, whether it is supported but with some caveats, or whether it is unsupported or questionable. Heuer Jr. and Pherson suggest using these three categories, however other practitioners use a 1 to 5 scale to rate confidence in an assumption, with 1 being totally unsupported conjecture, and 5 being an assumption with significant evidence supporting it. A 3 or less for any assumption should include a plan to gather additional information to help determine whether that assumption should remain part of the analysis - they are known as “key uncertainties” rather than key assumptions. 
    
7.  One additional suggestion, especially if there are several key assumptions low on the confidence scale, is to assign each key assumption a place on a 2 x 2 matrix measuring high and low certainty and high and low potential impact. Once they are all assigned a spot, based on group consensus, the low certainty/high impact assumptions are the ones to start trying to address most urgently.
    
    ![](assets/analyze_837710_02.png)

It is important to note that you cannot do a key assumptions check at one point in time and then hold the position that the assumptions will always remain valid (or invalid, based on your findings). A key assumptions check does not turn an assumption into a fact or a piece of evidence, it remains as assumption, which means that at some point additional information may make it more or less valid. It is important to recheck your key assumptions periodically, for as long as they still remain central to your analysis. 

### Analysis of Competing Hypotheses

Analysis of Competing Hypotheses, or ACH, is a method developed by Richards Heuer that is used to evaluate multiple, alternate hypotheses and identify the most likely hypothesis based on the evidence. ACH is an eight-step process that aims to force an analyst to look at all possibilities rather than to identify a hypothesis based on intuition and look for evidence that supports that hypothesis. The eight steps are as follows:

1.  Identify the possible hypotheses to be considered. Heuer recommends using a group of analysts with different backgrounds and different perspectives to brainstorm the possibilities. It is also important to differentiate between an unproven hypothesis and a disproven hypothesis during this step. An *unproven* hypothesis is one where there is no evidence that it is correct, whereas a *disproven* hypotheses is one where there is evidence that the hypothesis is incorrect. Include an unproven hypothesis, no matter how improbable, in the ACH process, but do not include disproven hypotheses.
    
2.  Make a list of significant evidence for and against each hypothesis. If you have already gone through the process of evaluating key assumptions, this step should be relatively simple; you already have the key pieces of evidence that contribute to the various hypotheses.
    
3.  Create a matrix with hypotheses across the top and evidence down the side and evaluate whether each piece of evidence supports or refutes each hypothesis. See [Figure 5-3](#ch05.html#fig_3_ach_matrix_template_completed_ach_matrix) for an example of this matrix. There are several approaches to filling out the matrix. Richard Heuer suggests listing whether each piece of evidence is consistent with the hypothesis by putting a *C*, if it is inconsistent with the hypothesis by putting an *I* or if it is neutral or not applicable, in which case you would put *N/A*. Others suggest using a weighing scale such as a plus sign (+) if a piece of evidence moderately supports a hypothesis or two plus signs (++) if it strongly supports a piece of evidence, and so forth.
    
    ![](assets/analyze_837710_03.png)
    
    In the matrix in [Figure 5-3](#ch05.html#fig_3_ach_matrix_template_completed_ach_matrix), we can see that H3 is not supported by any piece of evidence, and therefore would be removed from the matrix. Likewise, E3 is not applicable to any of the hypotheses, and E5 is inconsistent with every hypothesis, so the analyst would need to go back and reevaluate those pieces of evidence to make sure that they are both relevant to the analysis and are accurate. It is possible for a flawed or biased piece of evidence to make it through the evaluation stage, and this is another opportunity to identify whether something should be considered in the analysis.
    

4.  Conduct initial analysis to refine the matrix. After step 3, the matrix should show a few things: some hypotheses may have Inconsistent (I or -) assessments for every piece of evidence. Although this doesn’t disprove a hypothesis, if there is no evidence to support it, then it should be removed from the matrix (see [Figure 5-3](#ch05.html#fig_3_ach_matrix_template_completed_ach_matrix)). Similarly, if a piece of evidence has an N/A assessment for each hypothesis, the analyst should either remove it from the matrix or if it truly is a key piece of evidence, should reevaluate whether there is another hypothesis that needs to be considered.
    
    One thing that incident responders may run into from time to time is evidence from a separate, unrelated intrusion ending up in the analysis, simply because it was identified around the same time as the other evidence. When something does not match up with any other evidence, it may be best to remove it from the current analysis and analyze it on its own.
    

5.  Draw initial conclusions about the likelihood of each hypothesis. Focus on disproving the hypotheses rather than proving them to be correct. After the matrix has been initially refined, you can evaluate the likelihood of each hypothesis based on how much of the evidence supports it. In the example in [Figure 5-3](#ch05.html#fig_3_ach_matrix_template_completed_ach_matrix), H1 has the most supporting evidence, and if it is deemed by the analyst that E5 is not a valid piece of information, then there is no evidence that contradicts this hypothesis. H1 would therefore be considered the most likely hypothesis. H2 and H4 both have evidence that supports them, but also have evidence that is inconsistent with them; therefore, they are less likely. If any of the evidence that was marked as inconsistent proves that those hypotheses are incorrect, they would be considered disproven hypotheses. It is easier to disprove a hypothesis than it is to prove that a hypothesis is absolutely true.
    
6.  Analyze how much of your conclusion is dependent on a single piece of evidence. Re-analyze the information that led to your judgment of the most likely hypothesis or led to a hypothesis being disproven. Was there a single piece of evidence that weighed most heavily? If so, how confident are you in that piece of evidence? This will help determine the overall confidence in a judgment. If multiple pieces of evidence from various sources strongly support a hypothesis, there will be higher confidence in the assessment than if the judgment was based on one or two key pieces of information from a single source.
    
7.  Report your conclusions on the likelihood of all the hypotheses, not just the most likely one. It is important to record and report all the hypotheses that were considered, as well as the evidence that led to the final judgement. This is especially important if the analysis is going to go through a red cell analysis process, which we will discuss later in this chapter. It can also help to identify whether the analysis needs to be reevaluated if new information is provided, which brings us to the final step of the ACH process.
    
8.  Identify situations in which the analysis would need to be reevaluated. Richard Heuer writes that all analysis should be considered tentative, and that it is always possible for new evidence to be presented that will require a new analysis. If any intelligence gaps are identified or any information that you know is currently missing but has the potential to change the judgment, these should be documented to assist in future analysis. An example in the Road Runner intrusion would be the addition of information from another organization that experienced a similar intrusion, or information that the attacker’s activity was detected in the future due to additional security measures that were put in place. In those situations, you would have access to new log information. If either of these things happened, we would need to revisit the judgment.
    

The concept of setting predetermined criteria on when a judgement should be reviewed and potentially revised is a core component of the final core SAT that we will discuss next in this chapter.

### Indicator Generation, Validation, and Evaluation

In traditional cyber threat intelligence vernacular, indicators are equated with Indicators of Compromise, or IOCs, which we covered quite a bit in the first half of this book. In structured analysis, indicator does not just mean an IOC, although in some cases they are useful in this technique. Instead, indicator in this context refers to a set of observations that are used to track events, identify trends, or provide warning that something unexpected is occurring, or about to occur. In fact, there is an entire discipline around this called Indications and Warnings Intelligence, which is worth studying on its own if you find yourself frequently leveraging this structured analytic technique. 

#### Prepare an indicator list

The first step in this technique is to generate a list of the indicators that are used to identify that patterns that indicate a particular activity is or is not occurring. With Road Runner, we can capture the list of behaviors that, when combined, show us that our organization is being targeted. 

*   We are engaged in an activity that has historically been targeted by a variety of malicious actors
    
*   Malicious emails are identified that mimic activities unique to our operations, such as offering donations or volunteer support
    
*   Adversaries rely on exploit mechanisms that require user-interaction 
    
*   Members of our organization who are publicly identified are targeted more heavily
    
*   Similar tactics that we have seen are also used against other organizations similar to ours 
    
*   We receive warnings from government and other sharing groups about targeting
    

This list of indicators describes the status quo for our current operations, and a change in any of these behaviors could mean a change in the threats that we are facing. For example, a change in the type of suspicious or malicious emails we receive could indicate that something in the threat landscape has changed, either our adversaries are switching to a new tactic that we should be on the lookout for, or we are being targeted by different actors which we should strive to understand. 

For each indication demonstrating the status quo, it is possible to generate an indicator that would let you know that something is changing, either with you or with an adversary. These would be indicators we would actively look for. As an example, our indicator list could look like this:

*   The campaign ends and we are no longer engaged in an activity that is heavily targeted
    
*   Malicious emails are identified with novel subject matter
    
*   Adversaries we have seen targeting us begin leveraging vulnerabilities that do not require user interaction
    
*   We see more widespread targeting of employees, including those who are not publicly affiliated with us
    
*   Other organizations, governments, or sharing groups begin reporting novel tactics and targeting we have not previously identified
    

This list can be as detailed as you would like it to be, and can include specific tactics being used or vulnerabilities being exploited, as long as you have a plan to monitor for them. In fact, an added bonus of generating an indicator list is that it can help to establish collection requirements for future monitoring efforts. Regardless of whether you prefer to monitor for the status quo remaining the same or for direct indications of things changing, the indicators that you generate need to be validated and evaluated before operationalizing them. 

#### Validating and Evaluating Indicators

One area where Indications and Warning style indicators and IOCs overlap is in the validation phase. The criteria of what makes a good indicator are relatively the same between the two. The indications need to be something that can be observed, either directly or indirectly, otherwise they will only be useful retrospectively. The indication needs to be reliable as a sign that something is either remaining consistent or is changing. If it’s presence or absence can be influenced by various unrelated factors then it is probably not a good thing to monitor. Finally, it should be specific and point to one thing that can be monitored or observed, rather than a combination of factors which may change independently. For example, in our indicator list above we listed out the fact that malicious emails mimic expected campaign activities as one indicator, and then the fact that the exploitation requires user interaction as a separate indicator. While these two indicators both describe the phishing emails that we received, they are two different factors that could change independently and should be monitored individually. 

Similar to ACH, indicators can be evaluated by the criteria above and categorized as either ideal indicators that are highly likely to identify activity in the situation or scenario that they were designed for, and are highly unlikely to indicate alternate activities, or non-diagnostic indicators which could be observed in many different unrelated scenarios. Many indicators will fall somewhere in between, and alternate scenarios or possibilities should be carefully captured and recorded to support analysis if needed. 

After you have done all of the indicator generation, validation, and evaluation, it is important to remember that you are NOT DONE. You need a develop a process to monitor for these indicators on a regular basis, whether that is weekly, monthly, or some other regular interval. As with all of the core techniques we have covered, you should also schedule some time to evaluate your findings regularly, ensuring that it remains valid and useful. When reevaluating, it can be helpful to leverage some of the structured analytic techniques specifically designed to test previously established findings and seek to disprove them. When you have already established a viewpoint on something you may need some additional tools to avoid biases and remain objective, and contrarian techniques provide those tools.   

### Contrarian Techniques

The final type of structured analytic technique to cover includes several methods that are considered to be *contrarian*, meaning they seek to go against the existing standards or norms by providing a different view of the situation. In some cases, the initial analysis is conducted using contrarian methods, whereas in others contrarian techniques are used to question an existing judgment to ensure the analysis stands up in all possible situations. This type of analysis is not required in every case, but is strongly recommended for analysis for which a wrong judgment would have serious consequences, or where the judgment is expected to be contested.

#### Devil’s advocate

Devil’s advocate is used to challenge a widely accepted analysis of a situation by taking an opposing viewpoint and evaluating whether the available evidence truly disproves the alternative point of view. The devil’s advocate approach does not truly intend to prove that the alternative viewpoint is correct and the accepted analysis is wrong. Instead, it looks to expose any weaknesses in the original analysis, uncover any biases that were not accounted for, and help the original analysis stand up to intense scrutiny.

\[Add Example \] 

#### “What if” analysis

This type of analysis attempts to introduce new variables into the situation to see how that would change the analysis. For example, “What if this key piece of evidence is part of a deception campaign by the adversary?” or “What if this log data has been tampered with?” Again, this type of analysis does not attempt to directly disprove a hypothesis, and in fact can help with assessing confidence in an overall judgment by determining whether the analysis is sound even when certain pieces of intelligence are called into question. This technique can be useful in step 6 of the ACH process, as the analyst determines how much of the analysis relies on one or two pieces of evidence.

#### Red team analysis

This technique seeks to analyze how an adversary would think or act in the given situation. The analyst tries to put themselves in the mindset of the attacker and asks questions such as, “What is important to me in this situation?” and “What actions would cause me to deviate from the plan?” When conducting red team analysis, it is critical that an analyst take on an adversary’s persona. This technique can help counter mirror-imaging or mirroring bias by forcing an analyst to identify when an adversary’s mindset would differ from their own. Red teaming helps to identify additional factors that an analyst may not have initially considered.

Red teaming is a well-known concept in the information security industry, and it is important to know that this type of analysis relies on an understanding of an adversary, including basing the decision on the adversary’s social, political, and cultural tendencies, as opposed to some red team exercises that take a “just do whatever works” approach.

### Futures Wheel

A professor of mine described history as “And one thing led to another”. It’s a simple way to think about it, but has a lot of elegance to it. In most cases a decision doesn’t just have a singular outcome, but leads to multiple other outcomes. The Futures Wheel technique is a forecasting technique aimed at not just asking “What are the possibilities?” but asking “What could happen if this happens?” It works by creating an outline from an event, real or predicted, and then create a set of branches off of that of hypothetical possibilities. The key though is to consider what each of those possibilities entail and restart the process, taking each possibility and creating a new set of hypothetical possibilities. This spanning lets you see how one set of decisions could lead to unexpected possibility and better choose your decision consider 3rd, 4th, or nth level effects!

## Target-Centric Analysis

Structured Analytic Techniques are incredibly useful for giving analysts detailed, often step-by-step methods for working through different analytic problems. Just like the different models we have presented throughout this book; they can be leveraged on their own on alongside other analytic methodologies. We will detail another methodology that is useful in intelligence-driven incident response, target-centric analysis. 

In the book *Intelligence Analysis, a Target-Centric Approach*, Robert Clark identifies a new take on the traditional intelligence cycle that focuses on analyst-customer and analyst-collector relationships. By emphasizing these relationships, the process attempts to build and maintain a collective mental model of the situation to make it easy for customers to ask questions and easy for analysts to provide actionable intelligence. 

Clark writes that, “the target-centric process outlines a collaborative approach for intelligence collectors, analysts, and customers to operate cohesively against increasingly complex opponents. We cannot simply provide more intelligence to customers; they already have more information than they can process, and information overload encourages intelligence failures.” He further articulates that recent intelligence failures are rarely failures of not having the right information, they are primarily the result of failure to share information, failure to analyze information objectively, and failure of customers to act once intelligence has been shared. Most analysts have experienced each of these three at some time in their lives, and they tend to leave a lasting impression. Target-centric intelligence aims to address these failures by removing silos between roles and reducing the time in between each step of the traditional intelligence. 

An example of the target-centric intelligence analysis process is shown in [Figure 5-4](#ch05.html#fig_4_).

![](assets/analyze_837710_04.png)

Unlike Structured Analytic Techniques which detail specific methodologies, at its core, target-centric analysis is a conceptual model built around an entity that is being analyzed. A *conceptual model* is a representation of a system that helps others understand what the system entails and how it operates - many of the models that we discussed in Find, Fix, and Finish are conceptual models, including the Kill Chain and the Diamond Model. The goal of a conceptual model is to build an accepted representation of what is being modeled so that it can be discussed among different stakeholders and support collaboration. Since those are two of the primary goals of target-centric analysis, the model becomes a key part of the process. 

There are a few steps that can help you get started with target-centric analysis:

Step 1
: 

Get buy-in from the other stakeholders for changing the way that things are done. If you have typically operated in a different fashion with little direct collaboration between roles, then this will be a significant change. Since effective target-centric intelligence involves input from decisions makers, take the time to outline the business case for why you are starting a new approach and how it will benefit the organization. You can use the example of how more consistent collaboration between incident responders and threat intelligence analysts has supported an effective incident response process, and how this new approach to intelligence will help to extend that even further. 

Step 2
: 

Begin developing the conceptual model of your “target”. Many, though not all, targets are adversary networks, and the goal of understanding them is to determine how to best prevent, detect, and respond to any malicious activity you may experience from them. Depending on the adversary you may have specific information about who and where they are, however that is not necessary for a good model. Without any identifying information, you can still capture and describe a threat actor’s recurrent patterns of actions, understand the key components of their operations against you, and detail the behavioral, physical, and functional nodes that they rely on when attempting to compromise your organization. Building this shared understanding of the threat should lean heavily on information that has been exploited throughout the process thus far. It is one of the many reasons that the Exploit phase, as tedious as it may feel at some times, is so important. Developing the initial model for target-centric intelligence analysis can also leverage several structured analytic techniques, including things like brainstorming and key assumptions checks. 

Step 3
: 

Once the model has been developed, you also need to outline the operating processes you will follow to ensure that you are truly conducting target-centric intelligence analysis which is collaborative, iterative, and at times, predictive. Although the process needs to involve other stakeholders such as you intelligence customers, collectors, and other analysts, take ownership of driving the process and finding a way to engage other teams without putting too much of a burden on them - this is one way to ensure that you can maintain a collaborative approach. Outline logistics, such as weekly syncs to go over findings and identify new questions. Include an internal chat channel for asking time sensitive questions, and identifying secondary points-of-contact for when a primary stakeholder is out of the office or unable to participate due to other obligations. These may not seem like exciting tasks but they are critical to the success of this approach. 

Step 4
: 

Now we get to work. Once the conceptual model has been developed and standard operating procedures outlined, we begin the process of understanding the questions customers have around the model, the answers we already have, and the answers we still need. The answers we have are considered *actionable intelligence*, intelligence that can be acted on or can answer questions. Actionable intelligence is shared with the customer, and if more or different information is needed, the model is updated, and we move back through the collection and analysis process again. Target-centric analysis follows an iterative process, where information is gathered, analyzed to see whether it helps answer the questions presented, and occasionally ends up generating new requirements. If more or different information is needed, the collection and processing phases are reengaged, ensuring that all the necessary information is available to collective group. This method also allows for analysis to check in frequently with customers to see if the situation and needs have changed. Although every analyst wants to think that they can come to analytic judgements and conclusions while they are still needed, it is important to be honest about time constraints and how quickly things may change, which can alter intelligence requirements.  

Step 5
: 

Repeat as needed, and be open to change. The benefit of using the target-centric method of analysis is that it involves multiple check-ins between the various stakeholders involved in the process. Although you may have a few customers who are regular parts of the target-centric approach, it is beneficial to check in with all the recipients of the analysis, whether that is a CISO or a SOC analyst, to make sure that what is being provided meets their needs, and identify whether or not their needs have changed. This is a good way to not only ensure that the shared understanding that comes from the analysis continues to grow, and to identify when a customer may become a key stakeholder who should participate in the process more frequently.

Once you’ve identified the method that you will use for the analysis, but before you begin the analysis process, take a step back and look at the context for the question you are answering and the situation you are analyzing. As we discussed at the beginning of the chapter, this pause is a key step in switching from System 1 to System 2 thinking. It can be instinctual for an analyst to begin to formulate a hypothesis and start moving on to later steps - like how to communicate their findings - without ever conducting any formal analysis. This is your “fast” system 1 brain working (bless its heart) and stopping to thinking about additional context can activate the slow, deliberate thinking required in this phase of intelligence-drive incident response. 

# Conducting the Analysis

We spent the first part of this chapter building out the analytic toolkit that can be used to conduct analysis in a way that counters biases, is repeatable, and allows analysts to explain how they came to a particular judgement. Now the work can begin - as long as you know what it is you are analyzing.

## What to Analyze?

Conducting analysis is incredibly difficult if you do not know what you are trying to analyze. Often the thought of analysis conjures up images of someone standing in front of a wall with pictures and newspaper clippings haphazardly taped to it, waiting for that “aha” moment when everything suddenly makes sense. Or maybe you think of Newton sitting under a tree as an apple falls on his head, a simple (and over simplified) tale of an event that brings about sudden clarity, as everything you have been wondering about makes sense.

If your approach is to stare at all the data you have collected with the vague idea that you want to understand what it means, you will have a far more difficult time analyzing it than if you asked specific questions of the data, such as, “Why were we targeted?” or “How could this attack have been prevented?” You can certainly ask multiple questions of the information, and the questions can build on each other to increase your understanding of an attack and its implications, but without a starting point, many analysts find it difficult to complete this phase of the process. As we discussed at the beginning of this chapter, analysis often seems to mimic a mini-intelligence cycle, and therefore identifying or revisiting requirements - the questions you are trying to answer  - is the first step. 

If you do not have specific requirements that are being analyzed for leadership or other internal teams, you can ask a standard set of questions that are helpful for analyzing each incident. However, some questions will always be unique to your organization or incident, so do not think of the following examples as the *only* things you should be analyzing. Here are some of the questions you can start with:

### Why were we targeted?

This question will provide a wealth of information on how to identify additional intrusions as well as how to protect your organization from future attacks. The nature of the attack, whether the attacker targeted integrity, confidentiality, or availability of your data, whether they used the attack to get access to third-party connected networks, and the actions they took after finding what they were looking for, can all provide insight into what you need to be looking for moving forward. Tactics and techniques may change, but an attacker’s goals change far less frequently.

### Who attacked us?

This is often the first question that executives ask, but it is not the first question that we mention for a specific reason. Whatever it is that makes you a valid target for a specific criminal group may not be unique to that group; the same information may be targeted by another group with similar goals. Therefore, it’s good not to focus exclusively on the group that you happened to catch this particular instance and lose sight of the overarching threats. Once you do understand their goals, however, it can be useful to understand more about the particular attackers. Analyzing information about attackers can include things such as the tactics they employ, what they target, how careful and cautious they were, what hours they operate, what infrastructure they use, whether it appears to be an individual or a group, and any other patterns that can be identified by analyzing the data.

### How could this have been prevented?

A significant goal of analysis is to understand what happened and why it happened so that it can be prevented in the future. When answering this question, you will focus on the things that went wrong within your own network. Were there unpatched vulnerabilities that the attacker exploited? Were there IDS alerts that were triggered but that no one looked into? Did a user reuse a password that had previously been leaked as part of an unrelated intrusion? This is not usually a fun analysis to conduct, because no one likes to hear or see what they did wrong, but if your organization simply wipes a piece of malware from a system without understanding or addressing how it got there, then you will likely have to go through the whole incident-response process again, as the root cause was not identified or addressed.

### How can this be detected?

This is where all of the delightful indicators you collected come into play. After the sometimes painful process of analyzing how you could have prevented the attack, it is nice to know that you can put things into place to prevent or detect future attacks. What you are able to do will depend greatly on the security systems that you have in place. While answering this question, it is important to focus on aspects that are unique to this particular attack, such as malware hashes and command-and-control IP addresses, as well as the aspects of the intrusion that are not as ephemeral, such as the systems that were targeted or the tactics utilized as the attackers moved through the network.

### Are there any patterns or trends that can be identified?

This type of analysis is especially relevant when comparing internal incidents to incidents that have been reported either through information-sharing groups or through open-source channels. When answering this question, you can attempt to identify patterns at various levels—from patterns related to targeting of organizations that may indicate a campaign, to patterns that identify reused or shared attack infrastructure, or patterns in social engineering avenues used by an attacker.

The output of the analysis that you conduct in this phase should enable action, whether that action is updating a threat profile, patching systems, or creating rules for detection. Focusing on the preceding questions and any other questions or requirements specific to your organization, will help ensure that the work that you do in this phase will be able to cycle back into the operational phases of F3EAD.

## Enriching Your Data

Once you have identified the questions you are trying to answer through analysis, you may need to move onto the next step of the mini-intelligence cycle - identifying any additional collection that is needed. Fortunately, we have spent a great deal of time on collection throughout the intelligence-drive incident response process and we even exploited the data - aka organized, sorted, and made sure we could fully leverage it in analysis. At this point however, you may realize that to answer the specific question you are focusing on you need additional information about some of the data that has already been collected. This process is called enrichment, and is useful whether you are trying to answer tactically-focused questions around a particular malware family or are answering more strategic questions around why you were targeted. 

*Enrichment data* contains additional details about an indicator or a pattern of activity that usually aren’t used for detection, but for understanding more about a particular indicator and what it might mean if it is seen. This category of enrichment data can include things such as WHOIS information, autonomous system number (ASN), website content, recent and historical domain resolutions, associated malware, and many other additional details. The point of enrichment data is to gather more context around an indicator you have already identified so that you can better interpret its meaning. When looking at enrichment data in the analysis phase, you should focus on the patterns that emerge from the data rather than getting too caught up on one specific piece of information. One of the main reasons that many people end up with false positives and hundreds of thousands of indicators in block lists is that they take enrichment information and treat it as an indicator.

### Enrichment sources

The types of enrichment data that will be used depends on the indicator you are investigating and your goals for the analysis. Most enrichment sources provide information that is beneficial for multiple use-cases, but some are specific, so make sure that you know what you are looking for before spending a significant amount of time digging into a particular enrichment source. With all enrichment sources, it is key to record the date that the data was identified, as it is likely to change in the future. There is nothing more frustrating than finding a piece of information that is key to your analysis, and not being able to identify when or how you found it after it changes again!

Here are some of the types and sources of enrichment information:

#### WHOIS information

One of the most basic ways to get additional context and information about a domain or an IP address used in an attack is to get information about who registered or owns it. The WHOIS protocol, defined in [RFC 3912](https://tools.ietf.org/html/rfc3912), was originally intended to pass additional information about the users of the original internet, ARPANET. You could originally get WHOIS information through a command-line query, and that functionality still works today, though additional resources are available now, including websites and tools to capture current and historical data.

WHOIS has gone through several updates, as the user base grew and the scope of the internet expanded greatly. Currently, WHOIS contains information on the registrant’s name, email address, and additional contact information. WHOIS information can enrich analysis in several ways:

*   Tracking attacker infrastructure
    
    *   Some (not all!) attackers reuse information when registering domains. By identifying a name or pseudonym used by a malicious actor, it may be possible to identify additional malicious domains related to the same attacker group.
        
*   Identifying compromised domains
    
    *   In many cases, a legitimate domain may have been compromised and is being used by an attacker. Knowing the WHOIS information can help identify whether a domain is being run by an attacker or has just been compromised.
        
*   Identifying researcher-run infrastructure and sinkholes
    
    *   Many researchers on the internet carry out activity that looks similar to attackers, but is used for research or identifying vulnerabilities before actual attackers do. In most cases, the IP addresses used for this research will be identified through the WHOIS record, which can prevent an analyst from spending too much time digging into a nonmalicious IP address.
        

#### Passive DNS information

The original way that hosts on the internet were able to identify and communicate with each other was by using a single test file containing the names and IP addresses of all hosts. This file was aptly named *HOSTS.TXT* and was FTPed to all of the machines on the internet. This solution was sustainable when a limited number of hosts were on the internet, as shown in [Figure 5-5](#ch05.html#fig_5_arpanet). However, it was difficult to maintain, and as the file grew, it took more and more bandwidth to transfer.

![](assets/analyze_837710_05.png)

A more sustainable solution was developed, called the Domain Name System (DNS). It is essentially still a list of domains and hosts, but rather than a single file that is shared with everyone, the list exists on domain name servers that are queried when a host needs to look something up. The DNS is defined in [RFC 1034](https://www.ietf.org/rfc/rfc1034.txt) and [1035](https://www.ietf.org/rfc/rfc1035.txt), and [RFC 7719](https://tools.ietf.org/html/rfc7719) released in 2015 defines modern DNS terminology. *Passive DNS*, originally called *Passive DNS Replication*, is a technique invented by Florian Weimer in 2004 as a way to gather and reconstruct information from the global DNS. The original use case identified in the paper “[Passive DNS Replication](http://bit.ly/2tNacPc)” presented at the FIRST Conference in 2004 was the identification of domain names associated with botnet command and control IPs. Weimer noted that botnet C2 often uses multiple domain names rather than hardcoded IP addresses, and that these domains can resolve to multiple IP addresses, making filtering difficult. In order to identify what IP address a domain resolved to at any given time, or vice versa, would require collecting the information ahead of time and storing it in a database so that it could be queried when needed.

Passive DNS provides an analyst with information on IP addresses and domains that were identified during an investigation. It can also provide information on the nature of the activity. Passive DNS information is especially useful when paired with WHOIS information to get a more complete picture of an indicator of compromise. Just remember that Passive DNS information, like WHOIS information, is not static, so make sure that you pay attention to the time frame.

#### Certificates

The bases of both modern encryption and code signing certificates are another information rich source of information. Because certificates come from signing authorities that require more information than registering a domain or IP address, they have multiple facets to pivot off of. Different kinds of certificates (code signing vs network TLS) and different providers will collect different information. Given many providers set a high bar, both in information, validation, and cost, for getting certificates adversaries may be forced reuse certificates and makes a great pivot point.

#### Malware information

Information on malware can be extremely useful in analysis, and similar to Passive DNS information, many of the details around a piece of malware tend to change as time goes on and more information is discovered. Resources such as VirusTotal are living resources, the information changes as new entries are made, new detections are recorded, or users identify additional details around a sample. The following are examples of enrichment information on malware (Table 5-1):

Table 5-1. \[Table title to come\]

 
| Detection ratio || This number will change as time goes on and can be a useful indicator of the uniqueness of a sample that has been identified. When a sample first appears and is analyzed, the detection number, or the number of antivirus vendors who flag that sample as malicious, will be low. As time goes on, this number will increase. |
| --- | --- |
| File details || This includes information that has been identified about a file, which is updated as more individuals or organizations analyze the sample. Even if you have conducted your own analysis of a particular malware sample, seeing what other analysis has been conducted can help to fill in any gaps you have and can also indicate how widespread the deployment is. It is helpful to know whether something has been used only against your network, or is being seen across many networks in several industries. |
| Malware behavior || In addition to static information such as the hash of a malware sample, it is also possible to identify additional aspects of the malware’s behavior, including where it installs, other files it calls or relies upon to execute, and any automated or scripted actions it takes upon execution. These details can help you understand other malicious activities you may find on your network and provide a glimpse into the sophistication of the attacker and whether the malware is unique or is a variant of a common malware family. |

### Internal enrichment information

Not all enrichment information comes from external sources. Internal enrichment information provides additional details on things such as compromised hosts, users, or accounts. Internal information to be aware of includes the following (Table 5-2):

Table 5-2. \[Table title to come\]

 
| Business operations || Knowing what is going on in your network and in your organization at the time of an incident can help answer questions about why you were targeted and why the attack was successful. Did you recently announce new partnerships? Were you involved in a new merger or acquisition? These are important details that can help you understand the nature of an attack and often can be obtained only by talking to people within your organization. |
| --- | --- |
| User information || Identifying which users were targeted or compromised can help you understand what information the attackers may have been after, if you have not already identified what was stolen. It can also provide information on the attacker’s tactics; for example, if they initially target HR employees, and then attempt to move to a user with more access, such as a systems administrator. |

## Leverage Information sharing

Understanding when or if an indicator was previously identified can help put your particular incident into perspective. You should have identified some of this information during the Find phase, but it is useful to see what has changed or if any new information has been identified about any indicators that you are using in your analysis.

A good source of timely, nonpublic information of this nature is sharing relationships with other organizations. Public information is useful as well, and should be used in analysis, but information-sharing groups can provide details that often do not make it into the public domain, including specifics about when an indicator was seen, how it was identified, and what industry it was seen in. These are the types of details that many organizations do not want published but are often willing to share with partner organizations, especially when other organizations are sharing this sensitive information as well. There are formalized sharing groups, such as Information Sharing and Analysis Centers (ISACs), Information Sharing and Analysis Organizations (ISAOs), public/private partnerships, and informal groups. Many of the formal groups are arranged around industry or other shared interest groups. In some cases, information obtained from sharing groups can be used to detect malicious activity, but for the purpose of analyzing incidents, it can also be used as an enrichment source to help build out your understanding of the intrusion you are analyzing.

Once all information has been evaluated and enriched, it is time to move on to the next step: developing a hypothesis.

As you go through the process of collecting information, you most likely subconsciously start to form a hypothesis about whatever question you are trying to answer, which is the beginning of analysis. Analysis involves taking all of the information you have and synthesizing and interpreting it in order to determine its meaning and ideally what should be done about it. In order for analytic judgements to be complete, accurate, and reproducible, it is best to follow a structured process to conduct your analysis.

## Developing Your Hypothesis

At this stage, we begin to get into the actual analysis, which begins with clearly stating your hypothesis. As mentioned previously, you usually begin to come up with some working answers to your question during the collection/enrichment process. At the hypothesis development phase, you should start to document those working answers, no matter how speculative or far-fetched they are; the rest of the analytic process will weed out the obviously incorrect ideas. Make sure that when you document your ideas, you write them as fully as possible, and if specific information was found during collection that led to the formation of a particular hypothesis, make sure to note what it was. This will help with evaluating the hypothesis further down the road. If you cannot clearly articulate an idea, or you discover that it is too vague or doesn’t answer the question that you are asking, then it is not a good working idea, and you can move on to the next possible hypothesis.

In the case of our Road Runner intrusion, the first thing we want to understand is whether we were specifically targeted. Everything we have seen from this adversary indicates that they are a sophisticated threat group that is deliberate in their targeting, but we want to analyze the data we have to confirm that we were targeted. Our hypothesis, based on the information we gathered during the investigation and internal enrichment data about who at our organization was targeted, is that this was in fact a targeted attack aimed at obtaining sensitive information related to our campaign. This hypothesis is specific and is based on our research, but it still needs to be verified by going through the rest of the structured analytic process.

Over the course of your career, developing a hypothesis will become easier for several reasons. First, many incidents have similarities, and it will become easier to identify indications of a particular behavior. While this can cut down on the time it takes to carry out the analytic process, it is important to  *not* assume that this answer is correct; it is still just a hypothesis. Make sure that you go through the rest of the process even when the answer seems obvious, and make sure that you account for assumptions and biases, which we will discuss next.

The second reason that hypothesis development becomes easier is that after many iterations of this process as you step through numerous investigations, the mind often becomes more creative and less concerned when generating possible answers. After working through the analytic process and becoming comfortable with the fact that bad ideas *will* be identified and weeded out, an analyst can explore all possible ideas, often coming up with new ideas that they would not have identified before.

Regardless of how easy or difficult it is to generate your hypothesis, when you have a working hypothesis, the next step in the process is to evaluate the assumptions upon which that hypothesis is based so that you can move into the judgement and conclusion stage aware of and accounting for your biases.

## Evaluating Key Assumptions

As we discussed in the section on Structured Analytic Techniques, a *key assumption* is any part of a hypothesis that relies on a preexisting judgment or belief. Before continuing with the analysis, teams or individuals should take a few minutes or a few hours to identify these key assumptions and determine whether they are valid and should contribute to the analysis. For example, if an analyst has developed a hypothesis about how a particular attack could have been prevented, it is based on an assumption of how the attack was carried out, which would have been identified during the Find, Fix, and Finish phases. It should be relatively easy to evaluate whether that assumption is correct, but it should still be documented and discussed to make sure that all analysts have the same understanding of the information that contributed to the hypothesis. 

We outlined the SAT process for conducting a key assumptions check earlier in the chapter, however there are other methods for evaluating assumptions at different stages of analysis. The [CIA’s Tradecraft Primer](http://bit.ly/2vrrGmm) outlines how to evaluate key assumptions after hypothesis generation, and highlights the many benefits of going through this process. These benefits include developing an understanding of the key issues that contribute to a hypothesis, identifying faulty logic, and stimulating discussions among analysts. The process for evaluating key assumptions is as follows:

1.  Identify all key assumptions about a situation or hypothesis.
    
2.  Identify why the assumption was made.
    
3.  Assess the confidence in the assumption.
    
4.  Identify how the confidence rating was determined.
    
5.  Challenge each assumption and determine whether it is true and remains true for the current situation.
    
6.  Remove any assumptions that are not true or that have a low confidence; these should not be used in the analysis. It is important to note that you do not have to remove a hypothesis that has been generated because it was associated with an assumption that was removed, however it is important that there is still other evidence or other higher confidence assumptions in order for the hypothesis to remain plausible.
    

Our hypothesis that the Road Runner intrusion targeted us specifically is based on several assumptions. First, we are assuming that the actor who targeted us is Grey Spike, and that the activity we are calling Road Runner was this actor’s attempt to access multiple campaigns similar to ours. This is a key assumption, and it is made based on the fact that the information we obtained on the actor matched what we found in our network, including tactics, techniques, technical indicators, and targeting. We are confident in the accuracy of this assumption, based on the technical details and the timing. We are aware that additional information, especially information regarding deception activities on the part of the attacker, may change this assumption, and we will be prepared to change our analysis if any new information of that nature is identified.

Assumptions are not always easy to evaluate, and include things such as cognitive biases, which are logical fallacies or flaws in thinking that can easily cloud an analyst’s judgment. It is not possible to completely remove biases from analysis.

## Judgment and Conclusions

After the assumptions around evidence that supports a hypothesis have been evaluated and biases accounted for, the analyst can move toward making their judgment and conclusion about the hypothesis. Analysts can use several methods to interpret the evidence and identify whether a hypothesis is likely to be true or not, or whether they need to generate a new hypothesis either in part or in its entirety. Judgement is the step where analysts have to depart from what is known and use their knowledge, experience, and logic to fill in any gaps in known information to answer the questions that are facing them. Structured Analytic Techniques and Target-Centric Analysis can both help you get to a place where you know clearly the limits of your evidence and what it is telling you, understand your assumptions and conjectures and how confident you are in your validity, and have identified what new or changing information would impact those assumptions. At this point, analysts need to package this information as an analytic judgement. 

One common misconception is that a judgment is when you determine the correct hypothesis and answer the question being addressed as part of an intelligence requirement. While that may be the judgement that you present to the majority of your audiences during the dissemination phase, a judgement is actually required every time you determine that a hypothesis is incorrect as well. If you use a method such as ACH, then this step naturally flows from the process, as the later steps on ACH involve developing initial conclusions, which are often further evaluated using contrarian techniques. Even if you did not use ACH to conduct the analysis, a version of the final steps can still be useful to pull into any type of analysis:

*   Assess the likelihood of the hypothesis based on the evidence available. Focus on disproving the hypotheses rather than proving them to be correct. Identify if any information disproves a hypothesis. 
    
*   Review the information that led to your judgment of the hypothesis, whether it was accepted or rejected as a hypothesis. Was there a single piece of evidence that weighed most heavily? If so, how confident are you in that piece of evidence? This will help determine the overall confidence in a judgment.
    
*   Record your conclusion on each hypothesis, including citing the evidence that led to the conclusion and the confidence in the assessment. This should be directly tied to the evidence the supported or refuted it and should be clearly articulated so that another analyst would be able to review the evidence and clearly follow to logic. The rule of thumb here is that an analyst shouldn’t have to “be you” in order to understand the findings. 
    
*   Articulate what would have to change in order for you to determine that the analysis would need to be reevaluated. These criteria should become part of your indicator generation, 
    

A good formula for capturing a judgement is as follows:

I/we assess with *\[low, moderate, high*\] confidence that \[*state your judgment*\] based on \[*state the evidence reviewed*\]. I/we will be monitoring \[state your indicators\] to further support or refute this judgement. If you follow this method, even if you end up changing the statement during the dissemination phase, you will know that you have all of the pieces needed to make and support analytic judgement and conclusion. 

# Things that are Gonna Screw You Up, Aka Analytic Bias

We have covered several different ways that you can use structured processes and step-by-step approaches to analysis, all with the goal of engaging System 2 thinking and intentionally avoiding mental shortcuts that may lead to faulty analysis. System 1 thinking and our mental shortcuts are deeply ingrained however, and even using SATs and structured methods are not guaranteed to remove things like cognitive bias that can interfere with analysis. Understanding more about the mechanics behind these biases and some of the specific ways they show up in analysis can help identify them. 

Richard Heuer, one of the fathers of intelligence and the author of *The Psychology of Intelligence Analysis* (Military Bookshop, 2010) describes *cognitive biases* as “various simplifying strategies and rules of thumb to ease the burden of mentally processing information to make judgments and decisions.” Cognitive biases are essentially shortcuts that our minds develop so that we do not have to go through the entire analytic process for every minute decision we make in our lives. A basic example is a child who complains of being cold, and whose parents immediately tell him to put on a sweater. Years later, whenever that (now grown) child feels cold, he has the thought to put on a sweater. He probably also tells his children to put on a sweater. His mind does not have to run through the entire process of developing a hypothesis. (Maybe a hat would be best? Perhaps an extra pair of socks?) Testing and judging that hypothesis (clearly two pairs of socks are not enough) and coming up with a conclusion. He can simply shortcut to something that his mind tells them is an appropriate response to the situation.

## Accounting for biases

Cognitive biases aren’t always bad, and they do save a lot of time, but in intelligence analysis they can have a negative influence by causing an analyst to make assumptions and jump to faulty judgments. Another example of a cognitive bias at work is the use of previously identified malware, such as Poison Ivy. An analyst who has heard of or experienced several instances where sophisticated attackers used complicated, previously unseen malware, may automatically generate the assumption that this actor is *not* sophisticated. In this instance, they would be using a cognitive bias called *anchoring* that lets one piece of evidence override any other piece of evidence without a sound analytic judgement.

Many types of biases exist. Here is an overview of some that are commonly seen in intelligence analysis and incident response, along with ways that you can leverage the skills we have covered in this chapter to help counter them. 

### Confirmation bias

With confirmation biases, we tend to seek out or focus on evidence that supports our preexisting judgment or conclusion. If, in the back of our minds, we think that we are going to find evidence of a specific type of activity, any evidence that seems to support that conclusion will be given more weight than something that disproves or questions that judgment. In the Road Runner scenario, we may have run into the issue of an analyst who assumes that the actors were attempting to steal credentials, since we have seen the tactic in the past. If a phishing email appears similar to a credential-lure, the analyst may assume that as long as no password was provided that there is no lingering threat without investigating the actual email any further. That same analyst may also identify the use of password-guessing techniques, also used by less-sophisticated actors, to prove this assumption. This judgment would require the analyst to ignore the historical cases where sophisticated actors also used password guessing, or give them less weight. Confirmation bias is one of the primary reasons that it is important to go through the exercise of evaluating key assumptions prior to coming to a final judgment.

### Anchoring bias

In anchoring bias, analysts tend to become over reliant or give more weight to the first piece of information that they hear. Any subsequent information or evidence is compared to that initial piece of evidence, and the analyst often unconsciously debates whether the new evidence supports or refutes the first piece, making that first piece of information central to the investigation. If analysts were told going into the process of analyzing an intrusion that “we think it was Russia,” then each piece of evidence will influence their interpretation of whether it was Russia, when that really is not the question that the analysts were meant to answer. Anchoring bias is one of the reasons that some experts such as [Robert M. Lee ](http://www.robertmlee.org/tag/attribution/)say that true attribution (attribution to a particular government or nation-state) makes it more difficult for analysts to do their job because that attribution becomes the anchor that they base their judgments off of rather than a useful piece of evidence to include in analysis. Again, going through the process of focusing on the requirements and what question is actually being asked, developing a hypothesis, and evaluating key assumptions should help an analyst account for and counteract anchoring bias.

### Availability bias

In availability bias, there is an overemphasis on the information that is available, whether or not that information has itself been analyzed. Richard Heuer calls this bias the Vividness Criterion, which means that information that you personally experience or are most familiar with will become more important than information you are not as familiar with. It has also been referred to as the “I know a guy” bias, as in, “I know a guy who smoked a pack of cigarettes a day and lived to be 100; therefore, smoking can’t be that bad for you.” A newer version of the “I know a guy” bias is the “I saw it on the internet” bias.

Incident responders and intelligence analysts in particular need to be aware of this bias because this is what allows their previous experience to hurt rather than help them. If they focus on particular pieces of evidence that they are most familiar with because they have seen it before, then they may give that one thing too much weight or discount other pieces of evidence that they are not as familiar with.

### Bandwagon effect

Bandwagoning occurs when an assumption seems more likely to be true as more people agree with it. Although there is something to be said for group consensus after a piece of evidence has been analyzed, the bias comes into play when this judgment occurs prior to analysis, or when the fact that others support the assumption becomes a basis for believing that assumption is true. There is some interesting psychological reasoning with bandwagoning, and it can therefore be hard to overcome, but it is important to note that “because everyone says so” is not a valid justification for an assumption to be labeled as accurate. If there is evidence to back up a group consensus, it is important to look at the evidence rather than the fact that everyone is in agreement.

Heuer also refers to an “oversensitivity to consistency” and writes that, “Information may be consistent only because it is highly correlated or redundant, in which case many related reports may be no more informative than a single report.” To overcome this, Heuer recommends that analysts ensure that they are familiar with the body of evidence that previous analysis is based on, including the sample size and the information available, and question whether the same, consistent conclusion would likely continue with a larger sample size or the availability of more information. This is especially helpful with media reporting of an attacker. Multiple media reports may all be based on a single incident, so just because there are multiple reports does not mean that there were multiple incidents.

### Mirroring

Mirroring, or mirror-image bias, occurs when an analyst makes the assumption that a target being studied thinks like the analyst and therefore would make the same decisions as the analyst would. This leads to an analyst making assumptions about what an adversary would or wouldn’t do based on the analyst’s own personal experiences, which are often completely different from those of the target. Instead of using the evidence to guide judgments, an analyst suffering from this bias uses her own opinions of what would have been a logical step, or “what I would have done” to determine whether an assumption is correct. Mirroring is often used during the generation of working ideas, but during the Evaluation phase, it is important to identify when mirroring, rather than evidence, is the basis of an assumption and remove that bias from the analysis.

# Conclusion

If there is one phrase that can sum up the most effective way to approach analysis, it is *slow down*. Not only will this help engage the system 2 thinking that emphasizes deliberate, well-articulated reasoning, but will also help counter biases, know when and how to engage additional experts or get second opinions, and will actually result in a smoother and more effective process overall as you will make it easier for others to follow your reasoning in support of the judgement that was reached and begin to act sooner. 

 Analysts trying to answer specific questions about an incident or investigation do best when following a process that is both predefined but flexible enough to account for the complexity of the problems analysts often face. The process is not exactly the same in every instance—sometimes enrichment information is needed and ACH is conducted, and other times all the information is available and the analyst uses red teaming to evaluate a judgment—there *is* a process. Be flexible with your analysis, but be sure not to completely skip any of the steps; they are there to ensure that a sound analytic judgement is made based on the right information. After the analysis has been completed and a judgment made, it is time to decide how to best convey the findings to the appropriate audience, whether that is an executive or the security engineering team. We now get to ensure that all of the time you spent on analysis results in decision makers taking action. 

# Chapter 6. Strategic Intelligence

> “Our products have become so specific, so tactical even, that our thinking has become tactical. We’re losing our strategic edge because we’re so focused on today’s issues.”
> 
> John G. Heidenric

Once you’ve worked through the F3EAD process - from day one of Find all the way through Disseminate - you may be wondering what comes next. Well, in most situations, there is barely enough time to work all the way through the entire intelligence-driven incident response process before you need to jump right back to the beginning with another intrusion or some other pressing task. While it may seem urgent to move onto something else, you are not quite done yet. Taking a little bit of time to understand if and how the recent incident fits into your organization’s understanding of the strategic threat landscape is a task that will pay dividends down the road. It is one of the best ways to make sure that your organization actually learns from the incident and moves forward in a more resilient, informed manner.

All too often, incident responders find themselves dealing with the same situation manifesting itself in the same way. The same vulnerabilities, the same lateral movement, maybe even the exact same stolen or reused passwords, and very often the same adversaries. At that point, many find themselves shaking their fists at the sky, asking how this could have happened. Didn’t we learn from the last time? Didn’t we fix the problems? And unfortunately, the answer is often no. When the last incident was resolved, there were other things to worry about, other problems requiring the attention of everyone from the security team to the IT manager to the CIO, and since the problem had been “resolved” there was no more time to spend thinking about it. Lessons were not learned, and although some small changes may have been made, there was no lasting impact on the security of the organization because new, urgent problems took priority. Investing time in strategic intelligence, both understanding the “big picture” and assessing how individual incidents validate or change that understanding will help prevent incident dejavu and not only help protect your organization, but also help your security teams stay focused on emerging threats rather than the same threats time after time. In this chapter we will cover what strategic intelligence is, how it fits into the intelligence-drive incident response process, and how you can utilize it, whether you have a team or 1 or 100.

# What Is Strategic Intelligence?

Nothing happens in a vacuum - even (especially!) network intrusions. Everything happens within a specific context - the strategic environment - and that context holds a great deal of importance across the entire F3EAD process and beyond. The strategic environment extends far beyond your own networks, data, and the tactical and operational impacts of an intrusion. It includes geopolitical, economic, social, and technological issues, along with many other variables. Strategic intelligence provides the necessary information for planning future actions and policies and helps decision makers tackle long term projects and initiatives.

Strategic intelligence gets its name not only from the subjects that it covers, typically a high-level analysis of information with long-term implications, but also from its most common audience. Strategic intelligence is geared toward decision makers (most often business/agency senior leadership) with the ability and authority to act, because this type of intelligence should shape policies and strategies moving forward. This doesn’t mean, however, that leadership is the only group that can benefit from these insights. Strategic intelligence is extremely useful to all levels of personnel because it can help them understand the surrounding context of the issues that they deal with at their particular levels. Helping individuals understand why certain policies were created, or why an emphasis is being placed on a particular area will help them to fulfill their role more effectively.

In his paper, “[The State of Strategic Analysis,](https://www.cia.gov/static/789d2c84e758c9111bdaaa2adb8c628a/the-state-of-strategic-intelligence.pdf)” John Heidenrich wrote that “a strategy is not really a plan but the logic driving a plan.” When that logic is present and clearly communicated, analysts can approach problems in a way that supports the overarching goals behind a strategic effort rather than treating each individual situation as its own entity. When that logic is NOT there following any plan can become incredibly difficult, and knowing when the plan should be modified is even harder.

Strategic intelligence supports intelligence-driven response processes by helping analysts prioritize responses, identify when an intrusion is particularly significant to their organization, and ensure that the lessons learned from each incident are analyzed and acted upon. Without strategic intelligence, intelligence-driven incident response can still provide insight and support to the incident-response process, but with strategic intelligence it can drastically improve an organization’s ability to understand and posture themselves to prevent, identify, and respond to subsequent intrusions. But how does one *get* to strategic intelligence? How can you identify the logic that will help dictate the plan that you will follow, especially when there are so many other time-sensitive needs competing for your attention?

## Sherman Kent: Father of American Intelligence Analysis

Sherman Kent is known as the father of American intelligence analysis, and quite literally wrote the book on intelligence analysis with his book Strategic Intelligence for World Policy (still a great read almost 60 years later). He was so instrumental to the intelligence discipline that the CIA’s school to train new intelligence analysts is called the Sherman Kent School for Intelligence Analysis.

Kent received a doctorate in history from Yale University and taught on the faculty until World War II, when he joined a new division of the newly formed foreign intelligence service (and CIA precursor) Office of Strategic Services (OSS) called the Research and Analysis Branch (R&A). There, Kent used his experience as a historian, combined with an ability to lead, to bring together economists, scientists, philosophers, poets, and military members to conduct some of the most influential analysis of the war. Kent and the R&A analysts were not planning operations or tactical skirmishes; they were analyzing the underpinnings of the enemy and operating environment. They analyzed the cultures and the resources available (food, finances, and transportation) to help the United States determine what actions would have the most significant impact to the national strategy. They generated strategic intelligence that helped not just a single mission, but the entire war effort.

In many cases, one of the most significant differences between strategic and tactical or current intelligence is the modeling process. In tactical and current intelligence, analysts use the models that they have available, whether that model is an actor dossier or an internal network map, to solve the problem at hand. In strategic analysis, those models are often being updated or developed for the first time. In fact, when strategic analysts start leveraging only pre-existing models, whether mental or tangible, they run the risk of missing significant changes in the way the world functions making their analysis less effective, and in some cases flat out wrong.

While it is beneficial to have a corpus of work to look back on when applying these principles to modern intelligence-driven incident response, it is also important to understand how the world has changed since World War II and how that may change some of the doctrine intelligence analysts rely on.

## The Role of Strategic Intelligence in IDIR

The strategic intelligence of Sherman Kent’s day was designed to inform policy makers at the strategic level of what they needed to know in order to make the right decisions. Analysts often had access to information that no one else did, and were able to provide insight that would otherwise have been lost to policy makers. Things have changed quite a bit since that time, and leaders are often dealing with too *much information* rather than too little.

As we mentioned, strategic intelligence can be thought of as the logic behind a plan, and so the primary role of strategic intelligence in IDIR is often before or after an incident, rather than during, with a few exceptions. When leveraged before an incident, strategic intelligence can shape the response process itself, helping to identify how critical the response process is to an organization and whether there are any particular requirements to keep in mind - for example legal requirements about when and how to report the incident, and what type of data exposure warrants reporting. It can also help position defenses based off of an understanding of the strategic threat landscape and any significand world events that may have an impact on security. After an incident is the time when new information that was learned through the intelligence-driven incident response process is integrated into the existing understanding of the key questions that are often asked by leaders and executives, primarily things like:

*   Who is targeting our organization?
    
*   What are they after?
    
*   How effective are we at preventing attacks?
    
*   What is the impact of a successful intrusion?
    

It is important to note that while it is critical to include information from your own incidents to update the answers to these questions, they can also be influenced by intrusions that have occurred elsewhere that you learned about via threat information sharing or media reporting. If you are basing updates to strategic intelligence on media reporting, however, it is important to make sure that you have enough insight into what happened and trust the validity of the reporting before making significant changes to the way your organization views threats.

There are, as always, exceptions. While most often applied before and after, there are times when strategic intelligence can be directly leveraged during the incident response process. In these cases, analysts are often not generating strategic intelligence - after all the direct focus of incident response is often tactical in nature which requires a different way of thinking - but are instead turning towards strategic intelligence to help understand a potential contradiction that is stalling or even halting analytic progress. As Father Richard Rohr wrote, “Dictionaries define a *contradiction* as two things that cannot be true at the same time. I would say it this way: a contradiction is two things that cannot be true at the same time *by your present frame of logic.”* As analysts, we always view the information that we are analyzing through a specific frame, and when something does not make sense or does not seem like it should be possible, that often means the analyst needs a new frame. The process of reframing a mindset can be very difficult when remaining “in the weeds” (where many incident responders like to stay). Reframing, or changing your present state of logic, often requires taking a step back and looking to the bigger picture, which is what strategic intelligence provides. When using a structured analytic technique like a Key Assumptions Check, it can be beneficial to turn towards existing strategic level intelligence to help counter biases and identify information that may make the assumptions untrue.

## Intelligence Beyond Incident Response

This is as good a time as any to bring up a key concept that the threat intelligence world is still coming to terms with: intelligence can go far beyond just supporting detection engineering, intrusion detection, and incident response activities. Throughout this chapter we’re discussing strategic intelligence, how threat intelligence can be used to help an organization’s leadership make better decisions, but many other security decisions benefit from decision support.

### Red Teaming

The first way to use Intelligence beyond tactical/operational defensive operations is using it to inform adversarial operations. In red teaming an internal team acts as an adversary to challenge the intrusion detection and incident response teams. In many organizations the red team develops their own persona, often based on the red team’s engineers’ capabilities. If the team knows how to build zero days then the red team adversary will leverage zero days. If the red team engineers typically use the Metasploit Framework then the simulated adversary will use Metasploit. Even more complex is if the red team engineers know a programming language like Python then the adversarie’s tools will end up being written in Python.

You might be noticing at this point that the only adversary we’ve talked about at this point is the artificial one. That’s the problem. Giving the defense an adversary to track and respond to teaches them how to track and respond to that simulated adversary. But if the simulate adversary isn’t similar enough to the real adversaries an organization faces then the defense won’t learn useful lessons. For example, if the red team uses HTTP beacons over 80/tcp (because it’s easier to build or download sample remote access Trojans that just use well understood protocols like HTTP) but the organizations main adversary uses encoded in DNS then the tools, techniques, and procedures (yep, defense has TTPs too) the blue team develops responding to the red team may not be very effective.

So what way would be better? Well, the most obvious approach is for red teams to act more like real adversaries. Unfortunately, most red teams don’t know what real adversaries act like. This is somewhat expected. Most red teamers migrate out of one of three other roles which explain their... lack of perspective (sorry red teamers!):

Application Security Analysts
: 

They are focused on hunting through code for vulnerabilities. Some write fixes, some write exploits. Either way their approach is always going to be more focused on the vulnerability/exploit than how they’re really used in the real world.

Vulnerability Analysts
: 

Most of these folks focus on using tools like scanners to understand where there are network vulnerabilities. They know how to interpret the outputs of these tools, but don’t understand the kinds of objectives adversaries are after or the stages after initial exploitation.

Former Offensive Engineers
: 

While rare there are a small number of folks who were “real” offensive engineers. These folks, usually former government, have actually conducted intrusions. They do know what adversary tradecraft looked like, at least at one point in the past, but often can’t disclose it. Subconsciously they may emulate it, but not all nation state adversaries operate the same way, and chances are your new red teamers former employer is not your company’s current adversary (and if they are you may be in bigger trouble).

The fact is most red teamers will welcome being more relevant and useful. Certain immature red team members think their job is *popping shells and* *pwning* *boxes* but most realize that red team is really just blue team with a different focus. The goal is to help defense be as effective as possible. By taking the time to share and review adversary profiles that your organization faces the red team can adjust their TTPs to better emulate the threats your defenses will actually face, such as testing more effectively, using more realistic tools, and even setting up a similar infrastructure. All of these things help your defense prepare better for their actual adversaries.

### Vulnerability Management

Closely related to this is vulnerability management. Vulnerability management seeks to reduce attack surface by identifying, mitigating, and remediating vulnerabilities before an adversary can exploit them, usually by working with, or being a member of the organizations patching team.

Most of security operations is built on metrics, but vulnerability management more than most. Whether it’s a Common Vulnerability Scoring System (CVSS), the time since the patch was released, the criticality of the system, whether or not it’s a full moon, or some combination of all of the above! While the CVSS score does take into account things like whether or not there’s a public exploit, the idea that a known adversary is using it, how it’s being deployed, and what it’s being used to deliver can all make a difference.

For example think about Log4Shell attacks aimed at [CVE-2021-44228, CVE-2021-45046, and CVE-2021-44832](https://www.microsoft.com/en-us/security/blog/2021/12/11/guidance-for-preventing-detecting-and-hunting-for-cve-2021-44228-log4j-2-exploitation/). Initial reporting, largely from GreyNoise and CISA, identified the primary vector for Log4J exploitation as aimed at Java web services such as Tomcat and games like Minecraft. While these were 100% valid, Intelligence teams quickly started hearing about other ways it could be exploited, such as putting Log4Shell exploit strings in documents going through content proxies like web security gateways. That knowledge caused a fundamental shift in what systems the vulnerability management team needed to focus on, suddenly it was less about externally facing systems and moved to a much wider variety of internal systems. This was before the CVSS score was updated to take this into account.

Take a look at [CISA’s Known Exploited Vulnerabilities Catalogue](https://www.cisa.gov/known-exploited-vulnerabilities-catalog).

### Architecture & Engineering

Okay...those last two were pretty straight forward, but hear us out. There are actually a lot of ways Threat Intelligence can better inform network and even application engineering and architecture. In a lot of ways this ends up being an extension of our vulnerability analysis discussion: by understanding how adversaries are attacking systems we can better improve the resilience of those systems by whatever means make the most sense. Ultimately speaking systems engineering and architecture is a series of decisions about tradeoffs, and better information means better decisions, and intelligence is decision support.

A (Exceedingly) Brief Introduction to Resilience
: 

While a growing topic system, resilience is the ability for a system to resist external threats while returning the system to its ability to function effectively. According to Utah State University’s Center for Anticipatory Intelligence this is made up of the 4Rs:

Resistance
: 

How well do you keep the threat out?

Retention
: 

How can you keep the systems core functions going during disruption?

Recovery
: 

How quickly can you get the systems core functions to bounce back?

Resurgence
: 

How do you make a system stronger and smarter after the disruption?

Only one of these, resistance, is *left of boom*, before exploitation. The rest come after the adversary has started to gain control. The result is these come at the point where, through intelligence, we can use past knowledge to anticipate what steps an adversary will take and prepare to respond to them.

### Privacy, Safety, and Physical Security

Human systems have vulnerabilities, threats, and need resilience as well. Many of these challenges are amplified by technology, and while technology can’t solve these problems it is incumbent on us to build systems that don’t exacerbate them. This ends up being wildly specific to the system, and outside the scope of this book, but is certainly worth keeping in mind. It’s a different sort of system, but it can still benefit from the same kind of decision support from a strategic level threat intelligence.

Now that we understand more about the role of strategic intelligence in intelligence-driven incident response and beyond, we can move onto the hard part - actually conducting and leveraging strategic analysis.

## Building a Frame with Strategic Intelligence

Daniel Kahneman and Amos Tversky, whose [groundbreaking research](http://www.stat.columbia.edu/~gelman/surveys.course/TverskyKahneman1981.pdf) on framing and decision making has had a far-reaching impact on fields such as psychology and economics, advocate for building what they call a “broad frame” for making decisions. In a talk to the CFA Institute’s Conference, Kahneman urged that we should “see a decision as a member of a class of decisions that you’ll probably have to make” rather than a single, stand-alone decision.

A traditional “frame” in intelligence analysis is viewed like a picture frame on the wall. Everything you are looking at is contained within the frame, and the viewer is outside, looking in. Framing is a way of defining what it is you are looking at and to break a problem or an issue down into its component parts to be more closely studied. A frame often provides a visual means of identifying the individual parts of an issue so that they can be viewed independently. In this sense, we can think of many of the models we covered in Chapters 4 and 8 as analytic frames. With strategic intelligence, however, it is even more crucial that our frames be based on both analysis and synthesis. In addition to strategic analysis - or breaking down large-scale concepts into their individual parts so that they can be understood, we also need to think critically about *strategic synthesis -* or understanding how different pieces work together in a complex system. As Josh Korbel explains in his paper “[The US Intelligence Community’s Creativity Challenge](https://nationalinterest.org/feature/the-us-intelligence-communitys-creativity-challenge-11451)”, complex environments are defined not just by their parts, but how those parts interact in often unpredictable and unanticipated ways. In complex situations, analysis alone will not help understand the situation in a complex system. For a truly complex system, in addition to breaking down an issue into parts, framing must also include information on how the different parts interact and under what circumstance will the parts impact other parts. This process is called synthesis, and is a crucial part of framing for strategic intelligence in an increasingly complex world. In fact, synthesis is one of the primary reasons that it is so important to include intelligence-driven incident response findings into strategic intelligence - it helps identify and call out those relationships between parts of the system and highlight the changing nature of the landscape in which an organization or business is operating.

Frames can be either mental frameworks for approaching problems, a descriptive way that the problem is presented to others, or as mentioned above, a visual representation of a system or situation. With strategic intelligence which is used by many people in many different roles ranging from analyst to CEO, a documented, often visual, frame is one of the best ways to keep all necessary parties aligned. In the next sections we will cover a few of the models that can help with framing in strategic intelligence.

### Models for Strategic Intelligence

In *Psychology of Intelligence Analysis*, Richards Heuer points towards models as a key method to overcome several shortcomings analysts face, including the limitations of working memory and the ability to clearly recall multiple details, their connections, and their often complex context at any given point in time. We have used models in several different ways throughout this book precisely because they are such good tools to support analysis. With strategic intelligence, we will use models primarily as a way to create a detailed visual representation of a problem to assist with framing, analysis, and synthesis.

#### Target models

In some ways *Target models* are the ultimate type of model for framing. If we carry forward with the art analogy, a target model is a landscape painted in a realistic fashion by an artist with an eye for detail. As a whole, the painting tells the story of a moment in time, with enough interesting details and components to help the viewer understand more about the situation the more they look. Stepping back from the art analogy, target models are representations of an area of focus. They can describe things such as a broad problem area (food insecurity), a specific entity such as a corporation or a nation, or even a process (the ransomware distribution lifecycle). The goal should be to show the component parts as well as the relationships between different aspects.

Developing these models can be time-consuming, and the larger or more complex a target is, the more complex the model will be. Don’t let that stop you from beginning - once developed, target models can be some of the most useful tools for understanding a particular issue, and can easily be updated when new information is identified - such as information gained from the intelligence-driven incident response process. The first step in developing a target model is to determine what it is you are hoping to explain or understand by creating it. If you are hoping to understand where in your organization’s network you see the most successful or attempted intrusions, you can start with a network infrastructure map, which can then be overlayed with information from recent intrusions. If you are looking at something more broad, for example, how cybersecurity issues are likely to impact emerging multinational markets, then your target map will likely be focused more broadly on the components of multinational markets, and then overlaid with examples of cybersecurity impacts for each of the components. In both cases it is perfectly acceptable to start with a hand-drawn map of what you are envisioning as a way to get started. In many cases getting started is the hardest part of the process. \[[Figure 6-1](#ch06.html#fig_1__image) to come\]

![](assets/strategic_intelligence_466966_01.png)

Once you have developed a working target model, the hardest part is done, however models are rarely static; they must be updated periodically to remain applicable and effective. In the case of an organization’s network infrastructure map, for example, it must be updated frequently to remain current. With organizational structures, it can be necessary to update them whenever there is a reorganization or when key leaders change or leave, which can occur almost as frequently as network changes. Developing models is an investment, but will provide a solid frame for future analysis, whether it is used in the next intrusion you are supporting or used to propose a plan to increase the security budget or staffing of your incident response or threat intelligence teams.

In addition to target models, which can represent nearly any topic, there are some forms of specialized target models that can be useful is specific situations.

#### Hierarchical models

Some sets of information, such as organizational structure, fit best as *hierarchical models*. These models use a parent-child relationships to illustrate the chain of command, or leadership structure, of a target. This can help an analyst identify all the components of a target that need to be further analyzed. It can also help identify any choke points or bottlenecks that could be significant. This information can be gathered from many sources, and needs to be updated periodically as personnel or organizational structures change. A hierarchical model is best used to display a closed system that is not likely to change based on outside variables.

[Figure 6-2](#ch06.html#fig_2_hierarchical_model) shows an example of a hierarchical model.

![](assets/strategic_intelligence_466966_02.png)

Hierarchical models are traditionally used to show personnel or roles, but one unique application of a hierarchical model is to use it to identify the data that is important to an organization and which departments are responsible for it. A hierarchical model for data includes the broad categories of data, such as financial information, customer information, and sensitive company information. Any information that is valuable to the organization and that an attacker may try to access or impact in some way should be identified. When taking this approach, it is useful to be as specific as possible with the types of data. Financial information may break down further into credit card information, payroll information for employees, and futures projections for the company. All of this data likely sits in different places within the organization, with different teams responsible for maintaining and securing. Owner information for each data type should also be built into the model. This type of model will help organizations understand the data they are protecting and where it resides, and can be used to identify which data types are most targeted over time. As with all of the models we discuss in this chapter, it can take some time to build an accurate model, and this should not be done during an active incident.

#### Network models

*Network models* are useful when representing the relationships or interactions between individuals or groups that are interconnected in a non-hierarchical way. Network models are commonly used to develop computer network diagrams, both of an organization’s own network and often of an attacker’s infrastructure. In his keynote talk at the SANS cyber threat intelligence summit, Dr. Chris Sanders spoke about the importance of “friendly intelligence” in addition to “threat intelligence”. Friendly intelligence includes intelligence about things that we are trying to protect, and in intelligence-driven incident response we are often trying to protect our networks. Having a well-defined, up-to-date, and accurate model of the network is invaluable for defenders, and should be a part of strategic intelligence work. In addition to building out the physical and logical connections of the network, it is also helpful to include additional details that are relevant to your organization’s operations, including any regulatory compliance requirements for any portions of the network, including international requirements if your organization operates globally.

From a threat intelligence aspect, a strategic-level threat intelligence network models have also been used to show the relationships between attacker groups as well as the relationships between victims of an intrusion. As more and more advanced opportunistic attacks take place, understanding the relationship between victims can help better understand the nature of the threat itself. Network models have to be updated the most frequently of any of the data types discussed here, because they have many moving parts that change quickly and often. [Figure 6-3](#ch06.html#fig_3_network_model_example) shows a sample network map.

![](assets/strategic_intelligence_466966_03.png)

#### Process models

*Process models* illustrate the various actions and decision points that make up a structured process. Process models are especially important when conducting analysis as part of a team, as it helps keep people on the same page as they move through the analytic process. The cyber intrusion kill chain is another kind of process model. In Chapter 4, Chapter 5, and Chapter 6, it was used to capture indicators of a specific incident, but the kill chain can also be used to document the steps that an attacker takes at a more strategic level to assist in the development of the target model. There are two approaches to take when designing process models that support strategic analysis. You can either look at the often undocumented processes or SOPs (standard operating procedures) that you are already doing and build them out into a process, or you can look through examples of process models used in different types of analysis and pick one that best fits your way of operating and adopt it as your own. Either option is fine - process models are meant to be useful, not to dictate how you work, so do not feel that you need to change an already good process just to fit a model someone recommends. We have found however, that looking through models that others have developed does provide a good opportunity for reflection and optimization of existing methods.

In addition to the kill chain, there are several other models that can be used as guides for developing a strategic intelligence process model. In 2020, after reviewing several process models in fields ranging from criminal psychology to meteorology, Nicole Hoffman introduced her model, *Cognitive Stairways of Analysis.* Nicole described her motivation for publishing her model in her [blog post](https://threathuntergirl.com/2022/11/01/the-cognitive-stairways-of-analysis/), “Everyone talks about analysis, but no one goes into detail about how to perform analysis”. The Cognitive Stairways model include “steps” that are taken by an analyst that follow a set pattern, however the pattern can be tweaked to fit whatever particular task you are undertaking. In her example for brainstorming - a key component of any type of analysis, she includes the following steps:

1.  Brainstorm/Generate Hypothesis/Think Steps - this step will help you determine what you are going to focus your analysis on - similar to your intelligence requirements (though not always that specific)
    
2.  Determine the Scope - identify the frame that you will focus on for your analysis - the context through which you will conduct your analysis.
    
3.  Key Assumptions Check - what are the assumptions you will take with you into the analysis - making sure that the assumptions are valid before beginning, or they will need to be tossed out.
    
4.  Compile Data/Quality of Information (QoI) check - Gather data relevant to the topic you are focusing on, while evaluating the source and completeness of the data.
    
5.  Clean Data/Omit Useless Data - In order to properly use the data in analysis, it needs to be in a format and used with a taxonomy that will make it possible for you to both leverage and reference it within your analysis.
    
6.  Exploratory Data Analysis (EDA)/Visualization and Regression - What does that data you have gathered tell you about a topic? Try not to focus too much on the general hypothesis or think steps you developed in step one - the goal of that step was to get you on the right path, but now you must look at the data, using visualizations and regressions if helpful, to understand what the data indicates. Findings in this step may require you to hop back down to step 4 again to gather more data, and that is perfectly fine.
    
7.  Confirmatory - THIS is the step where you check your analysis and findings against your original think steps or hypothesis to come to an analytic judgement.
    
8.  Disseminate - This is where you conclude your analysis and share your findings, which ideally includes updating any pre-existing target models that may be changed based on the new findings.
    

![](assets/strategic_intelligence_466966_04.png)

Process models can take many forms, and the most important thing is that you have a process model that works for you, is documented, and can be leveraged by others.

#### Timelines

*Timelines* are linear models that show the time-based relationships between activities. Incident responders are most familiar with attack timelines that show when specific actions were taken during an incident, but many other timelines can be helpful in an incident-response situation. Understanding the timeline from vulnerability discovery to remediation is useful to help a network defender know how long they will remain vulnerable to a particular attack and can help decision makers determine when they will need to act. A timeline indicating when different actor groups were seen using a particular exploit or tool can help an analyst determine the threat from that malware, as well as understand how quickly or slowly tool-reuse propagates after a tool is identified.

Visualizing the temporal aspects of various activities provides analysts with a framework for understanding how those aspects will impact their organization’s goals and objectives.

# The Strategic Intelligence Cycle

Chapter 2 covered the intelligence cycle pretty extensively, but the primary focus was on tactical and operational-level intelligence—following the cycle in order to respond to a specific and often immediate threat. At the strategic level, the intelligence cycle follows the same process, but each step (from setting requirements to dissemination) looks different than it does when dealing with an immediate threat. Let’s examine these differences.

## Setting Strategic Requirements

It may seem that setting requirements at the strategic level would be more vague than with tactical intelligence. With tactical intelligence, there is a specific threat to focus on, which helps direct requirements. That is not often the case with strategic intelligence; and when requirements are passed down, they are often something vague like “Tell me what we need to know.” Although the scope and the time frame are much larger, the requirements should still be specific.

Strategic requirements often follow the military concept of *commander’s intent*. Commander’s intent is what allowed large, dispersed units to make decisions about when and how to conduct operations. Using the models that were developed as part of the strategic process, the commander, or the CEO or CISO, can state their goal or objective (a.k.a intent) and trust that all decision makers are on the same page and will take actions that will support that objective without the need to micromanage. For example, if the intent is to ensure that a company is first-to-market with a new product, then making sure that the manufacturing schematics, marketing plans, and other sensitive information are not leaked is a task that would fall under the commander’s intent. Developing a model of the attackers who may target that sensitive information is a strategic intelligence requirement that is necessary to support the ability to protect that information.

Strategic requirements, unlike tactical or operational ones, have the luxury of time on their side. Requirements can be planned far in advance, allowing them to be larger or broader in scope, depending on the needs of the organization, and they can also specify timing or periodicity. For example, a strategic requirement may be to update a company’s threat model twice a year, or to analyze what new threats may impact a business if they move into a new market or a new geographical region. When setting strategic requirements, it is helpful to identify early on if the requirement is ongoing, when analysis needs to be completed, and how often the findings will need to be reviewed or updated. It is also important to periodically review standing strategic requirements to identify whether they are still relevant and necessary. Strategic requirements, just like tactical and operational ones, can become stale if they are no longer relevant. However, it can take much longer to realize that strategic requirements are stale.

## Collection

The type of collection we have focused on so far in this book has been centered around logs and external sources such as threat feeds and information sharing. Although these types of collections still play a part in strategic intelligence, the scope of what you are collecting and from where will greatly increase, which is pretty exciting for those of us who are intelligence nerds at heart. Depending on your requirements—which have been specifically called out, *right*?—you may find yourself pulling information on economic, political, and cultural sources, or any number of other sources. This type of collection will also be more extensive than tactical collection, where any information older than a few days or even hours may be obsolete. With strategic intelligence, you may be searching for information that goes back years in order to capture trends or look for changes. The following sections describe useful types of strategic information to collect.

### Geopolitical sources

*Geopolitical intelligence* provides information on what is going on in the world, including conflicts, alliances, tensions, and other factors related to international relations in a particular region. There was a time when many people, maybe even some of the people reading (or writing) this book, disregarded geopolitics when it came to incident response. It is possible to hack a network from anywhere, so why would it matter if conflicts existed in certain areas of the world? Well, it turns out that there are many reasons geopolitics are important for incident response. Although it is possible to access networks from anywhere in the world, it doesn’t mean that regional conflicts or international tension have no impact on intrusion targeting and planning. Numerous times over the past decade, understanding geopolitical intelligence has been critical to understanding and responding to cyber attacks. Here are some examples:

*   In 2008, the ongoing conflict between Russia and Georgia escalated with a series of DDoS attacks against the Georgian state government, specifically targeting the website of the president along with sites dedicated to communications and finance. Shortly after these attacks began, kinetic operations commenced, making this the first official case of joint cyber-kinetic offensive operations.
    
*   In 2011, public outcry arose against the beating and eventual death of a homeless man in Fullerton, California, by police officers. As the investigation and hearings against the officers were conducted, both the city and the police department were targeted with DDoS attacks against websites and other attempted attacks targeting city assets. Attackers were successful at bringing the police department’s website down on at least one occasion.
    

What is going on in the world, whether it is in our own backyards or across the globe, matters to strategic cyber threat intelligence. Understanding political climates, conflicts, triggers, and tactics of adversaries can assist in strategic planning.

Although it is normal to focus outward on international threats, some aspects of geopolitics are local and should be considered as well. Good sources of geopolitical intelligence are peer-reviewed articles, white papers, and assessments. For this type of intelligence, is it often useful to look for historical information as well as current information about a situation. Understanding trends and patterns is particularly useful with geopolitical intelligence, where history often seems to repeat itself.

### Economic sources

*Economic intelligence* is incredibly important to network defense. Economics, the study of the production, consumption, and transfer of wealth, is not just useful for situational awareness, but for understanding the motivations of many threat actors. The vast majority of intrusions are economically motivated, whether that involves stealing credit cards for direct monetization or stealing intellectual property for strategic economic gain, and economic intelligence sources can provide insight into an adversary’s motivations.

Economic intelligence sources vary, and can include information on how stolen information is monetized, the types of information that criminals target, the types of information that are being targeted for industrial espionage, and economics associated with nation states that are likely to target you or have targeted you in the past. Even with a broad understanding of economics, this type of information can help organizations understand the strategic threats that they are facing. A specialized knowledge can provide an even greater level of insight, though it is harder to find someone specializing in economics on a network security team.

### Historical sources

*Historical sources*, such as analysis of a nation’s tactics or priorities from a previous conflict, are another often overlooked aspect of intelligence analysis when it comes to responding to cyber threats. The internet is new, relatively speaking, so how could historical sources possibly support cyber threat intelligence? If we consider the cyber realm to be an extension of the physical world, any activities that take place in the physical world will likely end up manifesting in the cyber realm as well. Because of this, history becomes important. If we can understand how adversaries targeted organizations before the internet existed, we can begin to pick up on ways that they will attempt to achieve the same goals by using new tactics and new mediums.

This is one of the reasons that military doctrines ranging from Sun Tzu’s *Art of War* to Carl von Cluasewitz’s *On War* are so often quoted in cyber-security presentations. Just because they were written long before modern incident response does not mean that they are not relevant to the prevention and detection of attacks in this new domain.

Con men were operating long before email was invented, and many of the tactics they used were similar to modern phishing scams, which just use a new avenue for their schemes. One tactic to help integrate historical sources into strategic intelligence analysis is to look at the most common threats that an organization sees, whether that is phishing emails targeting employees or targeted intrusions aimed at obtaining corporate information, and then to look for how those attacks have been carried out in the past. The goal of this type of analysis is to identify any lessons learned or patterns that can help the organization better understand the threats and better posture itself to defend against those threats.

### Business sources

Strategic intelligence, when used to support business operations, relies heavily on an understanding of the defending organization’s business. Many security professionals struggle with supporting strategic-level business decisions because they do not take the time to understand the problems that the business is facing or what information is critical to operations. Without understanding the business, it is nearly impossible for intelligence analysts or incident responders to produce strategic intelligence that will help leaders make the best decisions for their organization’s security.

Like all things in security, business operations and priorities are not static, so it is important to continually update and gather new information as it becomes available. *Business sources* include information on the markets an organization operates in, competitors, challenges to the business, new regions or markets that the business is planning to expand into, key personnel changes, and other aspects of business operations that have been identified as significant.

In addition to these sources of information (geopolitical, economic, historical, and business), which are more strategically focused than other collection sources discussed in earlier chapters, it is also important to incorporate information from previous incidents into strategic analysis. Doing so allows the analyst to present a holistic picture of what is being seen in the network combined with additional insight into historical, political, and economic trends that may influence the threats that an organization faces. All of this information will be pulled together in the strategic analysis phase.

## Analysis

Analysis at the strategic level follows the same process described in Chapter 8. Requirements are clearly stated, collection and processing occur, and hypotheses are developed and tested by researching and reviewing evidence that supports or refutes them. Strategic intelligence teams, however, must analyze a larger and more diverse data set, and therefore a larger team with diverse backgrounds and experiences should be utilized. You should keep these points in mind when conducting strategic-level analysis:

*   The evidence to be considered will come not just from network information, as is often the case with incident response, but from many sources that the analysts may or may not have subject matter expertise or substantial knowledge of. In those cases, understanding the source of the information is especially relevant because the analysts will often end up taking the information at face value. Look for information that comes from peer-reviewed, reputable sources. If a particular piece of evidence is deemed to be a key during a process such as analysis of competing hypotheses, it is best to try to find more than one source that is reporting the same information.
    
*   Biases can run rampant in strategic intelligence, where there is often a smaller amount of tactical evidence and more evidence that is open to interpretation.
    

## Processes for strategic intelligence

Some specific processes are more conducive to strategic-level intelligence, and others are far less effective at this level. For example, the target-centric model, which is an asset to an analyst investigating an intrusion or working at the operational and campaign level, is not as useful at the strategic level because as we discussed, many of the target models are being developed during the analysis.

Several analytic models and processes stand out as particularly useful for strategic intelligence, including SWOT analysis, brainstorming, and scrub down.

### SWOT analysis.

*Strength, Weakness, Opportunity*, and *Threat* (SWOT), is a model that is commonly used in risk management. SWOT takes into consideration internal aspects (strengths and weaknesses) as well as external aspects (opportunities and threats). It also lends itself to strategic intelligence specifically around network security and defense, because in many cases it will identify big-picture problems and concerns that will need to be addressed. It requires that an organization have a solid understanding of its core competencies and where they excel, be honest and up front about the issues that they face, and understand the external threats that are facing them. The basic outline for SWOT analysis is pictured in [Figure 6-5](#ch06.html#fig_5_swot_analysis).

![](assets/strategic_intelligence_466966_05.png)

For example, if documentation indicates that 90% of successful intrusions into the network come from phishing emails, this demonstrates a weakness of the organization that needs to be addressed. Identifying strengths can help determine steps that can be taken to mitigate those weaknesses.

SWOT analysis is useful not only for determining an organization’s own strengths and weaknesses. It can also be used to analyze a foreign government, a criminal organization, or an attack group. To conduct this analysis, it is important to pull heavily from the research that was done in the collection phase. An important part of using this type of SWOT analysis is to look for places where an adversary’s strengths line up with your organization’s weaknesses. Those are places that need to be addressed.

### Brainstorming

Strategic intelligence analysis should not be the work of one individual. As we mentioned, it is helpful to have multiple analysts with different backgrounds focusing on identifying the issues that will have a significant impact on an organization moving forward. An analysis on past intelligence failures (an analysis of analysis, one might say) has found that many times an intelligence failure is the result of group think, which discourages creativity and thinking outside of the box. General James Mattis said there is no place for group think, especially when it comes to national policy ([http://washex.am/2trKeBt](http://washex.am/2trKeBt)): “The national security decision-making process, as you know, you need different ideas to be strongly argued. You don’t want the tyranny of consensus of group-think.”

Brainstorming, especially with a group that comes from different disciplines, is a good way to counter group think by encouraging new and creative approaches to problems. Brainstorming can be used on its own or with nearly any other analytic method. Although it sounds as if it is unstructured, the CIA’s Tradecraft primer notes that brainstorming should be structured in order to be most effective. One of the most critical components of successful brainstorming is to allot enough time at the beginning of the process to enable the group to explore a wide variety of possibilities. When a group is time-constrained or feels rushed, they are more likely to select a smaller group of hypotheses that sounds realistic rather than exploring a larger set of possibilities that may generate new insight into an issue. It is also a good idea to ensure that at least one person is participating in the brainstorming who has a different role or approach than the team. Although getting together a group of incident responders to brainstorm will likely bring more than one point of view, it is still likely to be constrained to the typical experiences of an incident responder. By bringing in an outsider, whether that is a systems administrator, security architect, or someone from human resources, having new and different perspectives in the group will also discourage group thinking and force the rest of the team to consider new angles.

Brainstorming should result in the identification of a new set of hypotheses, and at that point the team can focus on identifying specific evidence from the collected information to support or refute the hypothesis, and then use an analytic method such as ACH to complete the analysis. It is just fine if it is not possible for the entire group to see the analysis to completion. One of the most important aspects of brainstorming is having the group there to identify new hypotheses, call out our unfounded assumptions, and identify bias at the beginning of the analytic process. If one or more analysts takes the lead on completing the analysis, it is still critical that they consult or check in with the group from time to time.

### Scrub down.

The term scrub down (also known as a murder board) was originally coined to describe a process used to help a candidate prepare for oral presentations. During a scrub down, an analyst presents his findings to a review board, which then questions not only the findings, but the analytic processes used to come to that conclusion. Through this process, it is possible to identify any biases that are present in the analysis, any key assumptions that were not validated, and any analytic leaps that were not founded in evidence. Even if the analysis is sound and no obvious errors exist, a scrub down helps an analyst vocalize the process used and explain the methods and findings, something that many intelligence analysts struggle with. When questioned about how a certain conclusion was reached, especially at the strategic level, that has more variables and pieces to tie together, an analyst will often default back to using vague terminology or anecdotes to explain how they conducted the analysis, and that type of explanation often does not inspire confidence in decision makers. Being prepared to describe not just the conclusion, but the analytic process itself, is a skill that takes time and practice to develop.

Rob Dartnall, an analyst from Great Britain, reminded us of the importance of the scrub down, especially when it comes to strategic intelligence where the stakes are high, in his presentation “Conventional Intelligence Analysis in Cyber Threat Intelligence ([https://www.youtube.com/watch?v=jzHw8lkocXA)](https://www.youtube.com/watch?v=jzHw8lkocXA\)).”

When analysis that you have done is about to lead directly to important and potentially drastic actions, it is imperative that not only is your analysis sound, but that you are prepared to defend it under scrutiny.

## Dissemination

*Dissemination* is only slightly different at the strategic level, and those differences are due to the scope and nature of strategic intelligence. The recommendations that are being made have the potential to significantly impact a businesses’ operations moving forward, and therefore, unless there are specific time requirements, accuracy and thoroughness take precedence over speed.

Many of the same principles that we discussed in Chapter 9 are directly applicable to dissemination at the strategic level, but there are some unique aspects, too. Specifically:

*   The audience is key at this level, and so it is important to identify who the audience will be before beginning the process of writing or creating the final deliverable. If multiple audiences would like to receive information in different ways, it is a good idea to make sure that the information is presented to each audience in the way that will be most useful to them. Make sure that the different versions of intelligence products or reports tell the same story, however. The last thing you want is for different leaders within an organization to have a different interpretation of the analysis and its meaning.
    
*   In strategic intelligence, it is important to specifically call out any intelligence gaps or trigger events that would result in a change to the analysis. It can be difficult to tell leadership that the analytic findings have room for doubt, but setting these expectations will make it easier to communicate changes as they occur.
    

# Moving towards anticipatory intelligence

As the saying goes “All models are wrong, but some are useful”. Strategic intelligence is ultimately a model and while it’s useful today it is not (and never was) perfect. As a result, like so many models, it will evolve or get replaced. At its core the United States Intelligence Community’s concept of strategic intelligence was based on a “two body problem” originating in the 1950s - The Cold War conflict between the United States and USSR. Nearly everything was looked at strategically as conflict between these two adversaries, even secondary and tertiary conflict was rolled up with the assumption that at a strategic level it was ultimately about the US vs USSR. The actions of the Vietnamese Viet Cong were really just a proxy for the Soviet intelligence and military apparatus. The 1970s Mujahideen in Afghanistan were a proxy force for the United State intelligence community. The modern era though is not nearly so straight forward. There are multiple sets of adversaries leveraging asymmetric threats (of which cyber is just one), and emergent problems like cypto-currency and climate change.

Strategic intelligence is designed to provide information to decision makers on long-term, broad topics with perceived implications to national security for years to come. It requires time to conduct solid intelligence analysis, and when a situation occurs before there was time for a complete analytic picture to be formed, decisions are often made without it. That often remains the primary focus even in a business decision making context, and more and more often results in actions with unintended consequences. As the world changes and needs change, many believe that it is time for strategic intelligence to change as well. An approach that may begin to make more sense for organizations and governments large and small is *anticipatory intelligence* - the practice of studying a situation and the surrounding context, environment, and related situations to anticipate future events and their implications. This is not the same as predicting a specific event - something that most analysts wish they could do regularly. While predictions are narrow and describe a specific event or outcome, anticipatory intelligence uncovers a broader spectrum of possible outcomes. Josh Kerbel, a strong proponent of moving towards anticipatory intelligence, proposed a new definition of anticipatory intelligence, “The intelligence process or practice whereby potentially emergent developments stemming from the increasingly complex security environment are foreseen via the cultivation of holistic perspectives.”

The good news is that moving towards anticipatory intelligence does not require an entirely new intelligence process - just the re-application of existing processes and practices, along with new ways of thinking about situations. This is a growing and developing field, but will be a critical component of intelligence-driven incident response in years to come.

# Conclusion

We consider strategic intelligence to be the logic behind the plan, and it is no wonder that many incident responders struggle with finding the time to conduct this level of analysis. In many organizations, incident responders would be hard-pressed to find a plan at all, much less understand the logic behind the plan. Strategic intelligence, when properly analyzed and adopted by leadership, can not only inform leadership of the long-term threats to an organization, but can also provide incident responders with policies and procedures that will support their ability to meet the needs of their organization.

Strategic intelligence for incident response not only enables you to make smart decisions about the visibility of your networks, but also feeds directly into the requirements for tactical and operational levels of analysis, and help you move towards anticipatory intelligence. It will help you answer the following:

*   Which threats are most significant so that incident response can prioritize and focus on those threats
    
*   Which types of information are important to capture, and what warrants a brief to the CISO or other executives
    
*   What situations may emerge as the outside world changes?
    
*   How do external situations such as financial crisis, global conflict, or pandemics impact the security posture of the organization?
    

There is a misconception that there is simply no time to conduct strategic intelligence. There is so much happening on a daily—and in the world of incident response, sometimes an hourly—basis, that many people feel overwhelmed by trying to keep up at the tactical level. Strategic intelligence, often viewed as a “nice to have” rather than a “need to have” gets relegated to the pile of things to do when time allows, and time rarely allows. However, strategic intelligence is critical to our ability to do our jobs, and although it does take time away from the daily emergencies, it can position us to better deal with those emergencies and therefore it should not be overlooked. All actions that take place can be tied back to strategic requirements. As you understand the logic and reasoning behind the requirements (and once you start adding to that logic yourself) you will be able to adapt and respond when situations change without having to revisit the entire process. Strategic intelligence takes time, but when it is done correctly, it can set up entire programs for success long into the future. It is well worth the time and effort.

# About the Authors

**Rebekah Brown** has spent more than a decade working in the intelligence community; her previous roles include NSA network warfare analyst, Operations Chief of a United States Marine Corps cyber unit, and a U.S. Cyber Command training and exercise lead. Rebekah has helped develop threat intelligence and security awareness programs at the federal, state, and local level, as well as at multiple Fortune 500 companies.

**Scott J. Roberts** makes up his title every time he’s asked, so we’ll say he’s the Director of Bad Guy Catching. He has worked for 900 lb security gorillas, government security giants and boutiques, and financial services security firms and done his best to track down bad guys at all these places. He’s released and contributed to multiple tools for threat intelligence and malware analysis. Scott is also really good at speaking in the third person.