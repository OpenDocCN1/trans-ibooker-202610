
## First Responders

As previously mentioned, if you are a first responder to a security incident, your first goal is to contain the incident. For example, if you are responding to a user’s complaint that their system seems to be slow and you notice after arriving that it may be due to a virus, disconnect the system from the network (by disconnecting the network cable from the computer) so that the virus does not infect other systems on the network. The only thing worse than dealing with an infected computer is dealing with multiple infected systems!

If you find that the systems on your network are replicating a worm virus out to the Internet, you may have to power off the entire switch or maybe even disconnect from the Internet so that the virus does not continue to replicate from your network out to the Internet.

> exam watch
> For the Security+ exam, know that the first responder should be focused on containing the security incident so that it does not become a bigger problem.
> 
> This may involve disconnecting the compromised system from the network to prevent the security incident from spreading across the network.

## Damage and Loss Control

The Security+ certification exam will test you on your knowledge of damage and loss control. With damage and loss control, the goal is to assess the damage during a security incident and then to try to control the losses due to the security incident.

The first thing you do when arriving at the scene is to assess the severity of the security incident. Ask yourself questions such as, “Is the incident affecting just one system or a number of network systems?”

The other goal is to control the loss. The best way to control losses from a security incident is to contain the incident by disconnecting the involved system or systems from the network. If one system is involved, you can simply disconnect that system from the network, whereas if a whole department of systems is involved, you may want to power off the network switch so no network connectivity can occur.

## Exercises

As part of your incident response strategy, it is important to partake in different types of exercises so that you are prepared when an actual incident occurs. You can use different types of exercises to help prepare employees for a security incident:

- Tabletop  With a tabletop exercise, the incident response team reviews the response procedures to follow when an incident occurs. This helps everyone understand their job role when an incident occurs.
- Walkthroughs  A walkthrough is more detailed than a tabletop exercise because it gives a security incident scenario, allowing the incident response team to discuss the steps to take in that scenario.
- Simulations  Simulations are actual events within a simulated environment to ensure that the incident response team knows how to use the tools and perform the steps needed to respond to an incident.

> exam watch
> Remember the different types of exercises for the Security+ exam. Note that a tabletop exercise involves a meeting where incident response procedures are discussed with the incident response team.

## Policies and Procedures for Incident Response

Organizations should take the time to prepare an incident response plan so that the company as a whole knows how to respond to security incidents. In addition to creating the incident response team and creating the incident response plan, you should also consider the following components when it comes to incident response policies and procedures:

- Stakeholder management  Understanding who the stakeholders are that are sponsoring the incident response project and understanding their goals in regard to incident response.
- Communication plan  Having a communication plan so that you understand the communicate channels to use within your organization. This includes communication channels to use with the stakeholders.
- Disaster recovery plan (DRP) and business continuity plan (BCP)  You learned in Chapter 18 about disaster recovery and business continuity. Be sure that a full disaster recovery plan and business continuity plan are in place.
- Continuity of operations planning (COOP)  Related to the business continuity plan is the continuity of operations planning (COOP), which is a plan that ensures all critical functions of a business can still operate during an emergency or disaster.
- Retention policies  Retention policies specify how long information is to be retained within the organization, including backups and offsite storage of data.

Many attack frameworks exist that can help you determine how to respond to incidents of attack. The following are a few attack frameworks mentioned in the Security+ objectives:

- MITRE ATT&CK  MITRE Adversarial Tactics, Techniques, and Common Knowledge (ATT&CK) is a framework designed to outline the phases of the attack life cycle and the common platforms targeted.

## INSIDE THE EXAM

### Incident Response Terms

In addition to understanding concepts such as the incident response team, incident response plan, and incident response process, you should know the following key terminology in relation to incident response:

**First responder** The first responder is the first security professional to respond to a security incident after it has been identified. The first responder’s primary objective is to contain the security incident.

**Recovery** A system involved in a security incident is considered a compromised system, and in many cases recovery involves wiping the hard drives, reinstalling or reimaging the operating system, and restoring data from the last good backup.

**Reporting** Documentation describing the incident and the lessons learned should be created to help educate the rest of the security team and organization.

**Quarantine** A compromised or affected system should be removed from the network until the security incident has been resolved.

**Legal hold** *Legal hold* is the term for putting data in a special hold so that users cannot delete that data during an investigation. For example, Microsoft Exchange Server allows an administrator to put a mailbox in legal hold so that the user cannot delete mail while the mailbox is being searched for evidence.

**Data breach** A data breach occurs when data that is considered sensitive is made available to an untrusted source. Be sure to control access to information and the use of external flash drives to help prevent data breaches in your organization.

**Damage and loss control** Each security incident requires identification of the extent of the damage and containment of the incident to control the loss associated with it.

**Strategic intelligence/counterintelligence gathering** An important task of the security team is to continuously monitor and log activity on systems and the network so that the team becomes aware of security events as they are occurring. Continuous monitoring is a never-ending process of monitoring access to resources, network traffic, and security logs for events.

**The Diamond Model of Intrusion Analysis** This threat intelligence model identifies the relationship between attack components of the adversary, capabilities, infrastructure, and victim.

**Cyber kill chain** The cyber kill chain was developed by Lockheed Martin and breaks down the attack process into seven phases: reconnaissance, weaponization, delivery, exploitation, installation, command and control, and actions on objectives.

## Data Sources to Support an Investigation

When performing an investigation or responding to a security incident, it is important to know where you can obtain information that can aid in the incident response. The following are common locations that can help with obtaining information:

- **Vulnerability scan output** You could perform a vulnerability scan of a system and look at the vulnerability scan result to identify security holes that may have allowed the attack to occur. For the exam, remember that the CVSS score reported with each vulnerability is an indication of the severity of the vulnerability. For example, a vulnerability with a CVSS score of 3 is not as severe as a vulnerability that reports a CVSS score of 9 (it ranges from 1 to 10).
- **Protocol analyzer output** You can also use a protocol analyzer or packet analyzer to analyze network traffic and look for anomalies in network traffic.
- **syslog/rsyslog/syslog-ng** You can use syslog to send logs from multiple sources to a central syslog server. You can use rsyslog on a Unix-type system to forward log messages to an IP network. You can use the new syslog feature of syslog-ng to implement extended syslog functionality such as content-based filtering.
- **journalctl** You can use journalctl, a utility to query journald's binary log data, so that the logs are readable.
- **NXLog** NXLog is a log collection and feature enhancement tool that runs on different platforms. You can use it to process logs as well as forward log events to other systems.
- **Bandwidth monitors** You can use a bandwidth monitor to identify anomalies in bandwidth usage to identify a potential network attack or malware on the network.
- **SIEM dashboards** You can collect information from a security information and event management (SIEM) dashboard, which is a central tool using sensors that collect security events and alerts that are generated by devices and applications on the network. The SIEM system correlates the events with security trends and allows you to adjust the sensitivity of the alert system.

> exam watch
> For the Security+ exam, remember the different information sources that can be used when investigating an incident. Be certain to remember that the CVSS score is a value from 1 to 10 that is reported with each vulnerability and represents the criticality of the vulnerability.
