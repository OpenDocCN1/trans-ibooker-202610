
Certainly frameworks are not intended to be completely exhaustive. Frameworks can be specialized in either an industry or a scope, or they can be broadly applicable. However, even those broadly applicable frameworks are not intended to cover 100 percent of your security controls. Take, for example, the NIST CSF, which is widely recognized as a general security framework that is applicable for basically any industry. Despite its breadth, the CSF does omit one huge security concern: physical security. Understanding that security frameworks are only a piece of the puzzle is an important part of selecting your security controls.

Lastly, remember not to confuse a framework with a standard or a control list. Frameworks, control lists, and standards are three different concepts. While a framework is an approach or strategy to take, a standard is a set of quantifiable directions or rules to follow. In most organizations, the standard would logically follow the policy, just as mandatory instructions would follow a decree stating what is mandatory. In any case, keep these differences in mind when selecting your organization's security controls.

What all frameworks have in common is an all-embracing, overarching approach to the task of securing systems. They break that task down into manageable phases or steps. For example, at the highest level, NIST SP 800-37, “Risk Management Framework,” breaks down the task of securing a system this way: tstarts with categorizing the system (based on the impact if that system's security were affected), identifying the proper security controls, implementing those controls, assessing their effectiveness, and then managing and monitoring the system. From this breakdown, security controls are the obvious central point involved in securing the system. These controls are the real tangible work performed from adopting a framework.

Once you have selected your framework, you need to review all of the controls to determine which are appropriate to address the risks you have identified. In some cases, the framework itself will provide guidance. For example, NIST SP 800-53 provides three levels of controls, called `initial control baselines`, based on the risk assessment. Threat modeling and risk assessment will also provide information on which controls are most important and need to be implemented with higher priority than other controls that address vulnerabilities with a low likelihood and impact. Implementing all of the controls in your selected framework to the same level of depth and maturity is a waste of time, money, and resources. The general objective is to reduce all risks to a level below the organization's risk threshold. This means some controls will have to be thoroughly and completely implemented while others may not be needed at all.

Often, the framework will incorporate a security standard with a minimum set of security controls forming a recommended baseline as a starting point for system security. Scoping guidance within the baseline identifies the acceptable deviation from control and results in a tailored system deployment that meets the organization's risk tolerance.

Implementing a defined, repeatable, and industry-accepted approach to selecting controls is one way your organization can demonstrate due care and due diligence in security decision-making.

Of course, selecting controls is just the beginning. You need to do the following:

*   Consider the control and how to implement and adapt it to your specific circumstances (the “Plan” phase)
*   Implement the control (the “Do” phase)
*   Assess the effectiveness of the control (the “Check” phase)
*   Remediate the gaps and deficiencies (the “Act” phase)

This Plan-Do-Check-Act cycle (also known as PDCA or the Deming cycle) is, of course, an iterative process. This was explicitly pointed out in ISO 27000:2009, but only implied in later editions. Other frameworks, such as the NIST Risk Management Framework, also leverage the continuous process improvement model to shape the efficiency and effectiveness of the controls environment. See [Figure 3.6](#c03.xhtml#c03-fig-0006).

![FIGURE 3.6 Plan-Do-Check-Act cycle](images/c03f006.png)

**[FIGURE 3.6](#c03.xhtml#R_c03-fig-0006) Plan-Do-Check-Act cycle**

On a more frequent basis, you need to (re-)assess the effectiveness of your controls, and on a less frequent basis (but still periodically), you need to reexamine your choice of controls. Business objectives change. The threat landscape changes. And your risks change. The set (and level) of controls that were appropriate at one point in time is unlikely to be the same a year or two later. As in all things related to information security, it is certainly not a case of set and forget.

In addition to the periodic review of the control selection, the following specific events warrant taking a fresh look at your security controls:

*   A security incident or breach
*   A significant change in organization structure or major staffing change
*   A new or retired product or service
*   A new or significantly changed threat or threat actor
*   A significant change to an information system or infrastructure
*   A significant change to the type of information being processed
*   A significant change to security governance, the risk management framework, or policies
*   A widespread social, economic, or political change (e.g., COVID-19)

The preceding list includes examples of “lessons learned” process triggers. It's highly advisable that your organization follow a periodic and event-driven process to evaluate your controls for suitability and effectiveness.

Security frameworks are also periodically updated, and when they are, one needs to consider the changes and adjust as appropriate.

## UNDERSTAND SECURITY CAPABILITIES OF INFORMATION SYSTEMS

The information systems that store and process your organization's data generally include an assortment of security capabilities that you should be familiar with. It is important that you understand these capabilities in order to properly evaluate them and effectively implement them. Some of the most foundational information system security (ISS) capabilities include the following:

*   Memory protection
*   Trusted Platform Modules (TPMs)
*   Cryptographic modules
*   Hardware Security Modules (HSMs)

### Memory Protection

One of the basic foundational security controls on all systems that allows multiple programs to run simultaneously is `memory protection`. This feature enables the operating system to load multiple programs into main memory at the same time and prevent one program from referencing memory not specifically assigned to it. If a program attempts to reference a memory address it is not permitted to access, the system blocks the access, suspends the program, and transfers control to the operating system. In most cases, the operating system will terminate the offending program. See [Figure 3.7](#c03.xhtml#c03-fig-0007).

![FIGURE 3.7 Operating system memory protection](images/c03f007.png)

**[FIGURE 3.7](#c03.xhtml#R_c03-fig-0007) Operating system memory protection**

A related hardware feature that is required to support memory protection is `dual-mode operation`. This means the processor can operate in one of (at least) two modes: privileged (or kernel) mode and unprivileged (or user) mode. The operating system runs in privileged mode, which grants it permission to set up and control the memory protection subsystem. Privileged mode also permits the operating system to execute special privileged instructions that control the processor environment.

Once a program has been loaded into memory and the memory protection is configured to limit that program's access to those areas of memory assigned to it by the operating system, the operating system transfers control to the program and simultaneously transitions to unprivileged mode.
