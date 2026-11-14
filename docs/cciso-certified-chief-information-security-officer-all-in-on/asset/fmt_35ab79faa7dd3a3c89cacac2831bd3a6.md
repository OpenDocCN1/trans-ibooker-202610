
# Chapter 1 Review Questions

## Question 2
**Answer: A.** Integrity of a system is maintained when only authorized changes are permitted. In this case the student is not authorized to modify her own grade.

## Question 3
**Answer: D.** A countermeasure is put into place to mitigate a potential risk. Also called controls, countermeasures are features such as devices, configurations, or products that reduce the likelihood that a vulnerability could be exploited.

## Question 4
**Answer: B.** Security policies define the hard-and-fast rules that must be followed. The security program plan describes the overall security program and organizational structure, while guidelines and standards are supporting documents that are important but do not define requirements.

## Question 5
**Answer: B.** The organization should prioritize having clear lines of authority. The CISO does not always need to report to the CEO as long as there is good communication. A matrix type of organization is just one of several viable approaches. The organization should be sized according to its tolerance for risk, not based on comparisons to other companies.

## Question 6
**Answer: C.** The Health Information Portability and Accountability Act (HIPAA) is a US federal regulation that focuses specifically on the handling of protected health information (PHI). HIPAA provides a framework for protecting the security and privacy of health information. The rules in HIPAA apply to covered entities and business associates.

## Question 7
**Answer: D.** The Payment Card Industry Data Security Standard (PCI DSS) is a control framework focused on the protection of credit card information. PCI DSS applies to any organization that handles cardholder data.

## Question 8
**Answer: A.** The EU-US and Swiss-US Privacy Shield Frameworks exist to regulate exchanges of personal information from the European Union and Switzerland to the United States. The goal of Privacy Shield is to enable US companies to receive personal data from the EU while complying with EU privacy laws.

## Question 9
**Answer: B.** The Open Web Application Security Project (OWASP) is an international organization focused on enabling organizations with the skills and tools to plan, develop, acquire, operate, and maintain secure applications. OWASP develops tools, guides, best practice documents, and organized conferences focused on application security.

## Question 10
**Answer: C.** The CISO must ensure that the organization is compliant with PCI DSS and SOX. The Sarbanes-Oxley Act (SOX) applies to all publicly traded companies doing business in the United States. The Payment Card Industry Data Security Standard (PCI DSS) applies to any organization that handles cardholder data. Since the organization is both a publicly traded company and handles cardholder data, the organization must be compliant with both.

# Chapter 2: Information Security Controls, Compliance, and Audit Management

## Chapter Topics
- Information security controls
- Information security control life cycle frameworks
- Information security control life cycle
- Exploring information security control frameworks
- Auditing for the CISO

## Introduction
[Chapter 1](#ch1.xhtml) introduced the concept of risk management, in which an organization performs a risk analysis of the entire enterprise to understand each asset's importance to the business. An asset is an information system, a facility, data, or any component (hardware, software, or data) used to support operations. This chapter explores how an organization goes about selecting and auditing security controls for a given asset. *Security controls* are processes or technologies put into place to address risks. Implementing a strong password policy, encrypting data at rest, and capturing events using logging are just a few examples of security controls. For a given information system or asset, hundreds of controls may be put into place. Organizations can develop their own definitions of controls; however, an approach that is simpler and less resource intensive is to use existing industry standard frameworks that provide libraries of controls along with guidelines for how to select them. The frameworks are typically used in a similar manner and generally follow the same process:

1. The first step is to categorize the asset or system using a model of defined categories. Usually the categories are defined by simply rating each security characteristic (confidentiality, availability, and integrity) of the asset with a security rating, which may be as simple as low, moderate, or high.
2. Based on the category of the asset, select the corresponding set of security controls that can be put into place to achieve the correct security. Frameworks often provide a method to map categories to suggested lists of controls.
3. Sometimes the organization tailors the set of controls suggested by the framework to the organization's specific needs and operational and business environment.
4. Once the controls are selected, they are then implemented, assessed, audited, and monitored for the life of the asset.

This chapter explores security controls, the security control life cycle, commonly used security control frameworks, and the auditing of controls.

## Information Security Controls
This section introduces foundational details about information security controls, including control classes, control functionalities, and types of control frameworks.

## Control Fundamentals
A security control, also known as a safeguard or countermeasure, is a mechanism put in place to mitigate risk and protect the confidentiality, integrity, and availability (CIA) of an asset. This section reviews fundamental terminology such as control class and control functionality.

### Control Classes
Controls can be grouped into *classes*, the names of which vary depending on the framework used. In practice, the three classes that are most often used are `administrative`, `technical`, and `physical`. However, NIST uses a different set of classes, `management`, `operational`, and `technical`. Different organizations use different terminology. Here is a description of the terms used most often:

#### Administrative
`Administrative` Management-oriented controls such as policies, procedures, guidelines, training, risk management, and employment practices (hiring, firing, and so on). Administrative controls are also referred to as soft controls or managerial controls.

#### Technical
`Technical` Hardware or software components that provide security control functionality. Examples include encryption, password enforcement, multifactor authentication, intrusion detection systems (IDSs), intrusion prevention systems (IPSs), and firewalls. Technical controls are also referred to as logical controls.

#### Physical
`Physical` Tangible controls put in place to protect people, assets, and facilities against physical threats. This includes protecting people, assets, and facilities. Examples include fencing, lighting, locks, bollards, server room doors, alarms, and security guards.

#### NIST Control Classes
As discussed, NIST defines a different set of classes. FIPS 200, *Minimum Security Requirements for Federal Information and Information Systems*, and NIST SP 800-53 Rev. 3, *Security and Privacy Controls for Federal Information Systems and Organizations* (and prior versions), use the control classes `management`, `operational`, and `technical`. Each of these classes are defined by NIST as follows:

##### Management
`Management` The security controls (i.e., safeguards or countermeasures) for an information system that focus on the management of risk and the management of information system security.

##### Operational
`Operational` The security controls (i.e., safeguards or countermeasures) for an information system that are primarily implemented and executed by people (as opposed to systems).
