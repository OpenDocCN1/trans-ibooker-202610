
The risk scenario depicted in *Figure 2.7* is part of the **Portfolio establishment and maintenance** category, and the scenario depicts an issue with resource allocation. As you can see, this risk is not security-related. Conversely, the following screenshot shows the same risk-scenario structure applied to a technical topic:

| Risk Scenario Title | Symmetric key compromised |
|---|---|
| Risk Scenario Category | Infrastructure theft or destruction |
| **Risk Scenario Description** | Symmetric keys could be compromised if stored improperly or used maliciously, leaving the door open to unattended access to Service Bus. |
| **Risk Scenario Components** | |
| **Threat Type:** | Malicious |
| **Actor:** | Internal, malicious insiders (most likely). External, Attackers (less likely). |
| **Event:** | Theft |
| **Asset/Resource (Cause):** | Process BAI10 is not well applied. |
| **Duration:** | Extended |
| **Occurrence:** | Timing is noncritical |
| **Detection:** | Most probably slow |
| **Lag:** | Immediate as the attacker can immediately authenticate against service bus |
| **Possible Risk Response** | |
| **Avoid:** | Use Azure AD to authenticate and prevent access to *Root/Manage/SharedAccessKey* |
| **Accept:** | N/A |
| **Share/Transfer:** | N/A |
| **Mitigate** | • Store keys in Key vault<br>• Renew keys frequently<br>• Monitor the *get secret* and *list key* operations |

Figure 2.8 – Technical risk scenario

The risk scenario depicted in *Figure 2.8* describes the possible malicious usage of a service bus symmetric key, which is used to authenticate against the bus. Parameters such as duration, detection, and lag help qualify the risk and identify the time between its occurrence and its consequences. These two very different examples show how you can use COBIT to your advantage for risk management. Both risks refer to COBIT's predefined categories and processes. You can extend COBIT within your categories and processes. Combining risk scenarios with the other COBIT tools, such as leveraging COBIT enablers, represents a powerful toolbox.

## COBIT wrap-up

As a software architect, you might be confronted with highly sensitive applications and systems. You could use risk scenarios since they are straightforward and nice to formally describe and respond to risk. We have not seen the other COBIT tools, but feel free to explore these further on our own. Now that I have briefly introduced COBIT, let's look at NIST.

## NIST

NIST is a well-known United States (US) framework (https://www.nist.gov/) that you can leverage for any technology-related topic. It ships with a cybersecurity-specific framework—namely, NIST Cyber Security Framework (NIST CSF) (https://www.nist.gov/cyberframework). NIST CSF covers all the duties I introduced earlier when describing the CISSP certification. As with COBIT for Risk, NIST CSF also handles risk management, but both frameworks can be used together. We will not look at NIST any further, but I wanted to make sure you keep it on your radar because the NIST organization publishes articles, reference architectures, and so on that might be useful for a software architect. I personally used such publications for cloud-related topics in the context of a cloud strategy exercise. To complete our framework journey, let's look at the indestructible ITIL framework.

## ITIL in a nutshell

ITIL is a United Kingdom (UK)-born framework. Back in the 1980s, the UK administration realized that its internal IT service management was rather chaotic. They decided to clean the house and build a framework that would assemble best practices from all over the place when it came to service management. Since then, ITIL, currently at version 4 (v4), is a world-leading IT service management framework. Infrastructure and operations teams around the world use ITIL, consciously or unconsciously.

ITIL primarily focuses on internal customers. For example, when you, as an employee or collaborator, request a corporate laptop to work with or when you need to access a certain system, you resort to ITSM. ITSM encompasses the organizational capabilities that deliver value to a customer (internal, in this case). The infrastructure department and the service-desk function are the back offices of a company. They keep the lights on and make sure everyone has the necessary tools and access to work. This is often thankless work but is necessary for a company to operate.

In 2018, ITIL v4 superseded the long-lived ITIL v3, which many organizations are still using today. ITIL v4 has changed a few concepts and extended the ITIL practices beyond ITSM capabilities. However, typical capabilities (request management, problem management, incident management, and so on) that all organizations need are still in scope.

As a software architect, you are more likely to contribute to application development to respond to **functional requirements (FRs)**. You should also be partially involved in NFRs (the subject of our next chapter), which include typical topics such as scalability, **high availability (HA)**, and so on. Many NFRs are not only code-related but also infrastructure-related, so you will *need* to collaborate with infrastructure architects. You might have experienced some mindset clashes and siloes between development and infrastructure teams. This is partly due to the way these teams are organized. Most development teams have adopted agile for a long time already, while infrastructure teams remain organized around ITIL. In ITIL, nothing is improvised; everything is thought through upfront and follows a clear life cycle and clear processes.

As a software architect, you should grasp the essential parts of ITIL to better interact with infrastructure architects. ITIL makes you understand that infrastructure teams deliver transversal services (mail, document management, network, corporate devices, and so on) that are scoped to the level of the organization, while most applications have a narrower scope. The scale of magnitude of these services partially explains why they rely on robust service management principles because they simply cannot afford an outage. In essence, ITIL is about service design and service operations. ITIL is about *measuring* and *managing* your IT services.

ITIL remains a fundamental pillar of the IT landscape, but DevOps and DevSecOps try to claim some space to make collaboration and toolchains more efficient across teams. ITIL promotes customer satisfaction, but real-world implementations are (too) often very IT-centric, while agile is more business-centric. The goal of DevOps and DevSecOps is to bridge these views, both of which are necessary.

## Summary

In this chapter, we browsed through some of the most widespread frameworks in the IT industry. You learned that TOGAF and ArchiMate are the languages of enterprise architects. As a software architect, you might be brought in to draw architectural patterns and some EA building blocks. We then reviewed NIST and COBIT for Risk, which come in handy to drive a security practice. We finally explored ITIL, the de facto ITSM framework used by most organizations. By understanding the essential parts of these frameworks, you should be able to optimize your interactions with other stakeholders. These extra skills might become a differentiation factor between you and an average software architect. Of course, I encourage you to explore some of the frameworks further, as these were only introduced in this chapter.

In the next chapter, I will introduce a software architecture methodology that I have been using at different places and that you will undoubtedly encounter sooner or later.

## 3
Understanding ATAM and the Software Quality Attributes

This chapter will provide you with an overview of the Architecture Tradeoff Analysis Method (ATAM), which is a widely adopted architectural analysis method used in organizations. As explained in the introductory chapter, software architecture practice may vary from one corporation to another or from one industry to another, but you should encounter ATAM on your way sooner or later. In this chapter, we want to make you understand ATAM’s essentials.

We will more specifically cover the following topics in this chapter:

- Introducing ATAM
- Understanding sensitivity points, trade-off points, risks, and non-risks
- Exploring quality attributes
