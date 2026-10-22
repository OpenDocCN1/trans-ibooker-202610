
## 2.6.3 Cybercriminal

Depending on the industry that you are working in, cybercriminals may be the most worrisome. Most organizations that are in industries like the financial or healthcare industries perhaps face the most difficult challenges when it comes to cybercriminals. There is one thing that motivates cybercriminals: Money.

As mentioned, many times, data is the new oil. It’s a commodity and organizations collect, trade, and monetize it. There is some folk lore that attributes a comment to Willie Sutton, a famous bank robber in the 1900s. As the lore goes, Willie Sutton was once asked why he continues to rob banks. His famous reply supposedly states, "Because that’s where the money is." Turns out that may not have been totally accurate according to Willie Sutton himself and was perhaps just an editor attempting to be pithy. Regardless, the statement is true and sums up, pretty well, why cybercriminals continue to target organizations and their data.

Their techniques vary. Ransomware, DDoS, data exfiltration, or even physical theft of devices. The goal is to get to the data, put it on the black market, and make some quick money. Their technical abilities vary, but they are more adept at infiltrating an organization from the outside than the previously mentioned actors. They will use off-the-shelf or purchased tools, or they can go as far as developing and customizing their own exploits to target specific entities. They are more targeted in their approach and will look for low hanging fruit in order to compromise an organization.

## 2.6.4 Hacktivist & Terrorist

I don’t mean to lump hacktivist and terrorist in the same category since they have different goals, but they are both propelled by the desire to publicly make an example of someone or some organization in order to advance an agenda. For both actors this tends to be of a political nature. But that’s where comparisons usually diverge.

Hacktivist will take up causes for people or movements aimed to effect change towards a shared goal. They are usually a loose band of individuals with a high level of skills. They will look to deface websites of organizations they don’t agree with or even DDoS them. Their goal is to raise awareness around a cause and their methods will vary.

Cyber terrorists move beyond political motivations and will often take up religious or ideological causes and target organizations accordingly. The targets can be individuals, or organizations and will often look to target key infrastructure of nations with the intent of causing physical harm.

## 2.6.5 Advanced Persistent Threat

Advanced persistent threats, or APTs, are gaining more recognition recently as nations such as Russia, China, North Korea, Israel, and the United States find their cyber differences (putting it mildly) out in the public space. APTs are deep pocketed, nation backed entities whose sole intention is to gain tactical and strategic advantage over adversaries. This shows up in their effort to gain access to organizations in specific industries, critical infrastructure, military entities, and so forth. In most cases, the APT will lie in wait and stay hidden until an order comes.

As mentioned, these are deep pocketed groups and are the highest skilled adversary that any organization will face. APTs are usually branches of an already established organization within the nation such as intelligence services or the military. Their focus is to gain a foothold into another nation with the intention of creating domestic chaos, take out military capability, or cripple a nation’s economy.

Defending against these types of attackers is not easy and the best advice is often to do the basics of monitoring and protection. However, Rob Joyce who led the NSA’s elite Tailored Access Group whose purpose was to break into adversaries’ systems, summed up every organization nightmare during a conference in 2016:

> We put the time in ...to know [that network] better than the people who designed it and the people who are securing it," he said. "You know the technologies you intended to use in that network. We know the technologies that are actually in use in that network. Subtle difference. You’d be surprised about the things that are running on a network vs. the things that you think are supposed to be there.

Rob Joyce

It’s important to highlight the last statement here. Systems are complex, and in any large organization the amount of applications and services running can be immense. Sophisticated attackers are counting on large organizations not having the bandwidth, personnel, or tools to detect malicious activity quickly. Their often right.

## 2.6.6 Why do we care?

Defending against these threat actors requires varying techniques. Organizations often find themselves as victims to broader geo-political attacks that have nothing to do with them. Especially when it comes to hacktivist, cyberterrorist, and APTs.

![](img/cc84747f93d290964598894b4f5c153b_56_0.png)

Figure 2.9 Relation of threat actors vs defense

Knowing your adversary is key to survival. Knowing that script kiddies will largely use already identified, automated attacks means that you’re off the shelf tools used for scanning

and protection will usually suffice. Defending against more sophisticated attackers like the hacktivist, cyberterrorist, cybercriminal, and APT mean that your defenses need to be more robust and a plan for business continuity is required in order to recover from a potential attack. For these types of attackers, you need to move from the preventative mindset and pivot to a detect and respond mindset. The more sophisticated the attacker is, the more resources are needed to defend. Regardless of the attacker, the basics of security need to be integrated. Scanning, patching, vulnerability management, visibility, and defense in depth are all required regardless of the threat actor.

## 2.7 Measuring risk

This book is not about risk, though it is an integral part of secure software development. There are plenty of resources available to help organizations understand and balance their risk. The goal of this section it to highlight some of the concepts with risk as it relates to an organization. Understanding this relationship will help application owners know why certain controls are used and needed. This leads to a stronger defense in depth model. Without knowing the risks that are posed you run the risk of overcorrecting for risks that are not of a legitimate concern.

As mentioned, there are many different methods for measuring risk. For the purpose of this book, we will use the Open Web Application Security Project (OWASP) methodology aptly named the OWASP Risk Rating Methodology that allows you to identify risks through a series of steps. It uses a number range from 0 to 9 to assign a value to a particular rating. The lower the number, the lower the level and vice versa. Although this is not a perfect system, the goal is to create a means for scoring security issues.

> **OWASP**
> Throughout this book we will leverage many of the projects from OWASP. When it comes to application security, OWASP is generally the first stop for most application security professionals. It is an open-source community of thought leaders that have built many of the foundational security practices used today. One of the most widely recognized application security projects is the OWASP Top Ten Web Application Security Risks. This documents the top 10 most impactful security risks that a web application faces. (https://owasp.org/)

Using the OWASP Risk Rating Methodology the measured risk comes down to a simple equation:

```
Risk = Likelihood * Impact
```

The likelihood is calculated by asking questions related to the threat agent and the found vulnerability. More on this in a bit. The aim of these questions is to uncover how likely an exploitation of the found vulnerability may be. With the impact calculation there are another eight questions that are geared toward identifying what will happen to the organization and technology should the exploit be successful. This will lead you to the ultimate rating of the risk which allows you to prioritize and apply the appropriate controls to eliminate or mitigate the risk properly.
