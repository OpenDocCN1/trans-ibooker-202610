
![](assets/cover_ER.png)

# Intelligence-Driven Incident Response

by Scott J. Roberts and Rebekah Brown

Copyright © 2023 Rebekah Brown and Scott Roberts. All rights reserved.

Printed in the United States of America.

Published by O’Reilly Media, Inc., 1005 Gravenstein Highway North, Sebastopol, CA 95472.

O’Reilly books may be purchased for educational, business, or sales promotional use. Online editions are also available for most titles ([http://oreilly.com/safari](http://oreilly.com/safari)). For more information, contact our corporate/institutional sales department: 800-998-9938 or `corporate@oreilly.com`.

- Acquisitions Editor: Jennifer Pollock
- Development Editor: Angela Rufino
- Production Editor: Elizabeth Faerm
- Copyeditor: TO COME
- Proofreader: TO COME
- Indexer: TO COME
- Interior Designer: David Futato
- Cover Designer: Karen Montgomery
- Illustrator: Kate Dullea
- October 2023: Second Edition

Add additional revdate spans below as needed.

# Revision History for the Early Release

- 2022-02-25: First Release
- 2022-08-18: Second Release
- 2022-11-18: Third Release
- 2023-02-07: Fourth Release
- 2023-04-07: Fifth Release

See [http://oreilly.com/catalog/errata.csp?isbn=9781098120689](http://oreilly.com/catalog/errata.csp?isbn=9781098120689) for release details.

The O’Reilly logo is a registered trademark of O’Reilly Media, Inc. *Intelligence-Driven Incident Response*, the cover image, and related trade dress are trademarks of O’Reilly Media, Inc.

The views expressed in this work are those of the authors, and do not represent the publisher’s views. While the publisher and the authors have used good faith efforts to ensure that the information and instructions contained in this work are accurate, the publisher and the authors disclaim all responsibility for errors or omissions, including without limitation responsibility for damages resulting from the use of or reliance on this work. Use of the information and instructions contained in this work is at your own risk. If any code samples or other technology this work contains or describes is subject to open source licenses or the intellectual property rights of others, it is your responsibility to ensure that your use thereof complies with such licenses and/or rights.

978-1-098-12062-7

[LSI]

# Chapter 1. Introduction

> But I think the real tension lies in the relationship between what you might call the pursuer and his quarry, whether it’s the writer or the spy.
>
> John le Carre

Once relegated to the secretive realms of national security and military operations, intelligence has become something that is fundamental to the daily functioning of many organizations around the world. At its core, intelligence seeks to give decision makers the information that they need to make the right choice in any given situation.

Previously, decision makers experienced significant uncertainty because they did not have enough information to make the right decisions. Today they are just as likely to feel like there is too much information, but just as much ambiguity and uncertainty. This is especially the case with network security, where there are fewer traditional indications that a significant action is actually about to take place. In order to make decisions about how to prepare for and respond to a network security incident, decision makers need analysts who understand intelligence fundamentals, the nuance of network intrusions, and how to combine the two into an accurate assessment of a situation and what it means for their entire organization. In short, they need analysts who can conduct intelligence-driven incident response.

Before diving into the application of intelligence-driven incident response, it is important to understand the evolution of cyber security incidents and their responses, and why it is so relevant in this field. This chapter covers the basics of cyber threat intelligence, including its history, recent activity, and the way forward, and sets the stage for the concepts discussed in the rest of this book.

## Intelligence as Part of Incident Response

As long as there has been conflict, there have been those who watched, analyzed, and reported observations about the enemy. Wars have been won and lost based on an ability to understand the way the enemy thinks and operates, to comprehend their motivations and identify their tactics, and to make decisions—large and small—based on this understanding. Regardless of the type of conflict, whether a war between nations or a stealthy intrusion against a sensitive network, intelligence guides both sides. The side that masters the art and science of intelligence, analyzing information about the intent, capability, and opportunities of adversaries, and is able to act on that information, will almost always be the side that wins.

### History of Cyber Threat Intelligence

One of the best ways to understand the role of intelligence in incident response is by studying the history of the field. Each of the events listed below could (and often do!) fill entire books. From the iconic book *The Cuckoo’s Egg* to recent revelations in decades old intrusions such as Moonlight Maze, the history of cyber threat intelligence is intriguing and engaging, and offers many lessons for those working in the field today.

#### The First Intrusion

In 1986, Cliff Stoll was a PhD student managing the computer lab at Lawrence Berkeley National Laboratory in California when he noticed a billing discrepancy in the amount of 75 cents, indicating that someone was using the laboratory’s computer systems without paying for it. Our modern-day network security-focused brains see this and scream, “Unauthorized access!” but in 1986 few administrators would have jumped to that conclusion. Network intrusions were not something that made the news daily, with claims of millions or even billions of dollars stolen; most computers connected to the “internet” belonged to government and research institutes, not casual users, and it was easy to assume everyone using the system was friendly. The network defense staple tool `tcpdump` was a year from being started. Common network discovery tools such as `Nmap` would not be created for another decade, and exploitation frameworks such as `Metasploit` would not appear for another 15 years. The discrepancy was more easily expected to be a software bug or bookkeeping error as it was that someone had simply not paid for their time.

Except that it wasn’t. As Stoll would discover, he was not dealing with a computer glitch or a cheap mooch of a user. He was stalking a “wily hacker” who was using Berkeley’s network as a jumping-off point to gain access to sensitive government computers, such as the White Sands Missile Range and the National Security Agency (NSA). Stoll monitored incoming network traffic with printers writing reams of packets onto paper to keep a record and began to profile the intruder responsible for the first documented case of cyber espionage. He learned the typical hours the attacker was active, monitored the commands he ran to move through the interconnected networks, and observed other patterns of activity. He discovered how the attacker was able to gain access to Berkeley’s network in the first place by exploiting a vulnerability in the `movemail` function in GNU Emacs, a tactic that Stoll likened to a cuckoo bird leaving its egg in another bird’s nest to hatch and inspiring the name of his book on the intrusion, *The Cuckoo’s Egg*.
