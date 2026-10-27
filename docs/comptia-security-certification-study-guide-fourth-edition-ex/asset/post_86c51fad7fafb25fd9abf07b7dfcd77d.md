
## CERTIFICATION OBJECTIVE 17.01

## Introduction to Risk Analysis

In this section you will learn about the need for risk analysis and the benefits that it offers to your organization from a security point of view. You will also learn about the general process to follow when performing a risk analysis.

## Risk Analysis Overview

Having the knowledge to perform a risk analysis, or assessment, is an important skill for a security professional because a number of different security concepts use risk assessments as a starting point. For example, a security assessment typically starts with you performing a risk assessment so that you can identify the resources that need security. Also, performing a business continuity plan involves performing a risk-type analysis called a business impact analysis (BIA).

The overall purpose of risk analysis is to identify the assets within the organization and their value so that you can then identify the threats to those assets. After identifying the threats, you determine how to take countermeasures against each of them (called mitigating the threat), while at the same time ensuring that your countermeasure does not cost more than the value of the asset! That is a key point to risk analysis—it would not make business sense to spend $10,000 on a mitigation solution to protect an asset that is only worth $2,000.

Before I introduce you to the risk analysis process, I want to list some terms that are used when discussing risk analysis. You should be familiar with the following risk analysis–related terms for the Security+ exam:

- **Asset** A resource that your organization needs to function
- **Vulnerability** A weakness in the configuration of hardware or software
- **Threat** An event that can cause harm to the asset
- **Threat vector** A tool, or mechanism, the hacker uses to exploit a weakness on a system
- **Threat actor** The person (hacker) using the threat vector to compromise the system
- **Threat target** The system or device being attacked
- **Risk** When the threat to an asset can cause harm to the organization—typically resulting in a financial loss
- **Risk analysis** The identification and planning of mitigation techniques to reduce and manage the risks to your organization

## Risk Analysis Process

It is important to follow a process when performing your risk analysis so that you are sure to identify all the threats and risks associated with your assets. Following a risk analysis process allows you to plan proper mitigation techniques to reduce the risks. This section outlines the process of risk analysis and breaks the process into different phases.

## Identify Assets

The first phase of performing a risk analysis, also known as a risk assessment, is to identify the assets within the organization and the value of those assets. This phase is also known as asset identification. For example, if a company earns revenue by selling products online, the web server hosting the e-commerce web site would be considered an asset to the company.

During this phase, be sure to focus on identifying the assets instead of worrying about things like the threats to those assets—that will come in the next phase. By focusing only on what the assets are, you properly identify all assets of the organization. The following is a list of common assets held by organizations:

- Hardware and software
- The organization’s information and data
- The building(s)
- Inventory and cash
- Personnel
- The organization’s reputation and branding
- Computer systems and services

When identifying your assets, watch for the hidden assets. For example, your e-commerce web site that is bringing in thousands of dollars a day typically has a database server that is holding the listing of products and the details of each of those products being sold.

## Identify Threats to Each Asset (Threat Assessment)

Once you have identified the assets in the organization, you then turn your focus to threat assessment, which involves identifying the threats to each of the assets identified in the first phase. Continuing with the preceding example, a company e-commerce web site has a number of potential threats; for example, the system could be hacked via a buffer overflow exploit or an SQL injection attack. The web server could also experience a hard drive failure, which could cause the system to be down for a long time, resulting in lost revenue.

When performing the threat assessment, it is important to identify threats from different sources—or different types of *disasters*. There are environmental threats, person-made threats, and both internal and external threats. The following identifies these different types of threats, also referred to as *risk types*:

- **Environmental** An environmental threat results from the environment, or location, in which your business operates. Environmental threats include, among others, floods, tsunamis, earthquakes, volcanic eruptions, tornados, blizzards, lightning storms, and hurricanes.
- **Person-made** A person-made threat, also known as a manmade threat, is a threat that does not exist naturally but instead is a result of human actions, whether intentional or unintentional. Examples of manmade threats would be viruses, fire, theft, vandalism, and sabotage.
- **Internal and external** It is important to note that threats can be either internal or external. An *internal threat* comes from someone inside the organization. This could be a threat such as a disgruntled employee intentionally deleting customer data, or an employee accidentally deleting a file. You need to protect from both types of threats. An *external threat* comes from outside the organization and could be, for example, someone on the Internet trying to hack into your mail server or your web site.
- **Legacy systems** The threat or risk of having a legacy system connected to the network is that the legacy system may no longer be supported by the vendors of the system's hardware or software. For example, the vendor may no longer create security patches for the legacy system, making it vulnerable to attacks. If you do require that legacy systems stay in production, look into placing them on their own segment.
- **Multiparty** A multiparty risk or threat is a risk to multiple people or companies. For example, if you lease a car, you are concerned about the risk to the car as the lease owner, but so are the leasing company and the insurance company.
- **IP theft** Another common threat or risk to organizations is the theft of intellectual property (IP). An example of this threat could be due to an insider within the organization who had access to confidential company data and took it with them when they left the company and went to the competition. You can mitigate the risk of IP theft by implementing encryption and access control policies.
- **Software compliance/licensing** Another major risk to an organization is in regard to software licensing and compliance. It is easy for large organizations to lose track of the number of software licenses they require, so it is important to perform regular audits to ensure that your company is in compliance with licensing requirements. Organizations not in compliance with licensing may face legal repercussions and fines.

It is important to understand that many threats come from the fact that weaknesses, or vulnerabilities, exist in the assets of the organization. For example, the vulnerability could be a system that has not been properly protected, which results in the threat of the system being hacked. The following outlines some common vulnerabilities that may exist:

- **No system hardening** It is critical to understand that a system that has not been properly secured, or hardened, helps create the threat that the system will be hacked.
- **No physical security** Having no physical security in place can result in a number of potential threats, from having the asset stolen to breaching data confidentiality.
- **No security controls on data** Having no security controls on your data means that you have threats relating to confidentiality and to tampering with data.
- **No administrative controls** An administrative control is a policy or procedure. Not having policies and procedures in place can result in the threat that you are not compliant with regulations or standards.

Various types of threats could exist for any number of assets that your organization may have. The threats may vary from being technical in nature to being more related to actions performed by personnel. The following is a list of common examples of threats:

- **Theft** Theft is a huge threat nowadays because you are dealing not only with physical theft of an asset but also with digital theft of information assets.
- **System hacked from inside** You could have critical systems or services on the network compromised by an internal employee, whether intentional or not.
- **System hacked from outside** You could have public servers such as web, FTP, and SMTP compromised by someone external to your network (Internet).
- **Natural disasters** Natural disasters such as flooding, hurricanes, and tornadoes are examples of threats against the business continuing its operations.
- **Hardware failures** When running servers that hold critical data or functions, it is important to identify hardware failures that could occur. The hardware could be hard drives, network cards, or even machinery in a production plant.
- **Fraud** This is a bit trickier to identify, but watch for threats of fraud because employees could be tampering with information or company processes to gain a financial benefit.

The Security+ certification exam expects you to know the risks associated with technologies such as cloud computing and virtualization technology. Cloud computing is great in concept, but it means that someone else is hosting the services, which could lead to a violation in confidentiality. For example, if you are hosting your mail on a provider’s system, you have no idea who has access to that system and whether they are accessing your corporate mail. Be sure to implement high levels of auditing in such environments.
