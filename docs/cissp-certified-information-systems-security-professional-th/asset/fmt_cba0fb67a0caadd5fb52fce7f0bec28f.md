
**ISO/IEC 19770-2:** Establishes an ITAM data standard for *software identification* (or SWID) *tags*, which provide authoritative identifying information for installed software or other licensable items. SWID tags allow your organization to uniquely identify software that's deployed on your hardware assets.

**ISO/IEC 19770-3:** Provides an ITAM data standard that establishes common terminology to be used when describing software entitlement rights, limitations, and metrics.

**ISO/IEC 19770-4:** Establishes a data standard that supports standardized reporting of resource utilization. This standard is especially important when managing cloud-based systems or other complex assets and licenses.

**ISO/IEC 19770-5:** Provides an overview of ITAM and defines related vocabulary.

### Configuration Management

Configuration management is discussed in detail in [Chapter 7](#c07.xhtml), but it's important to introduce here as it relates to asset management and asset security. Related to keeping the inventory current, system and software configurations must be tightly controlled and thoroughly documented — that's the role of *configuration management*. Configurations made without direction can lead to problems in other security areas, like vulnerability management, because hardware and software requirements for upgrades and patching might be unknown and missed.

There are two types of baselines with which you should be familiar. A *system baseline* identifies the versions and settings of all configuration items (CIs) in a product, system, or subsystem; system baselines answer the question “what do I need to build the system correctly?” A security *baseline* is a minimum set of safeguards (e.g., security controls) required to protect a given system. Organizations commonly establish a baseline for every category of system within their enterprise (for example, they may establish a Windows baseline, a RedHat Linux baseline, and a macOS baseline) to describe the minimum acceptable configurations that must be in place for each of these systems. Many organizations use guides or checklists to accomplish and maintain a secure baseline configuration for their assets. A leading source that is publicly available is the National Checklist Program (NCP), defined by NIST SP 800-70. This is the U.S. government repository for up-to-date and detailed guidance for security configurations. Similar guidance exists in other countries, for both broad and specific application. A popular example in the EU is the “Baseline Security Recommendations for IoT” guidance that establishes a set of recommended configurations for critical information infrastructures.

Some organizations automate much of the baseline security configurations through the metadata available in the NCP and using the *Security Content Automation Protocol* (SCAP). SCAP enables validated security products to automatically perform configuration checking using NCP checklists. [Domain 7](#c07.xhtml) of the CISSP CBK goes deeper into configuration management, and we explore the concepts of provisioning, baselining, and automation in [Chapter 7](#c07.xhtml).

### Change Management

*Change management* is an IT discipline focused on ensuring that organizations employ standardized processes to make changes to their assets. Organizations must develop policies and procedures to prevent arbitrary and unexpected modifications to their hardware and software inventory. Change management includes all activities that allow your organization to roll out and prioritize changes in a controlled manner that does not adversely impact your production environment or customer expectations. This includes change control (which is the set of processes and tools that allow you to verify that authorized changes are implemented correctly), enforcement (i.e., prevention of unauthorized changes), verification of changes, and audit.

If change management is not handled properly, a variety of issues can arise. For example, significant gaps can gradually emerge between your inventory record and the reality in your environment. Because any change to your IT environment can potentially impact your organization's security posture, all significant IT changes should go through a formal change process that includes an assessment of the security impact of the change. Change management is further discussed in [Chapter 7](#c07.xhtml).

## MANAGE DATA LIFECYCLE

Maintaining data security requires that you understand where the data is in the overall data lifecycle. [Figure 2.5](#c02.xhtml#c02-fig-0005) depicts the secure data lifecycle, and each step is described in the list that follows.

![FIGURE 2.5 Secure data lifecycle](images/c02f005.png)

**[FIGURE 2.5](#c02.xhtml#R_c02-fig-0005) Secure data lifecycle**

1. **Collect:** Data is generated or aggregated.
2. **Store:** Data is saved into a storage system or repository.
3. **Use:** Data is processed and/or analyzed, by users or systems, for its intended purposes.
4. **Share:** Data is shared with authorized external users and systems.
5. **Retain:** Data is kept (e.g., archived) for a predefined period of time.
6. **Destroy:** Data is deleted and permanently removed from storage, making it inaccessible and unusable.

The data lifecycle is a high-level process that describes how data can flow through an organization, and the specific steps may vary slightly from one organization to the next. For example, some data may go straight from step 1 to step 6, never being stored or retained, while some data may never reach step 6, instead being archived (i.e., retained) in cold storage forever.

Understanding the differences between each phase in the secure data lifecycle is critically important to define efficient security processes and effective data security controls. The data lifecycle model can be used to help your organization think through the security needs of your data during each phase. We discuss each phase and the relevant data roles in the following sections.

### Data Roles

A key factor in managing the security of your organization's data is being able to identify the parties that control or may access your data and understanding their roles and responsibilities throughout the entire data lifecycle. Some regulations (GDPR, for example) set strict requirements for organizations to identify specific individuals for specific data roles. The meaning of each title may vary slightly from one regulation to another or from one organization to the next. A general description of each data role is discussed throughout the rest of this section.

#### Owners

Not surprisingly, data owner is one of the most important roles in an organization. A *data owner* is an individual or group of individuals responsible for dictating how and why data should be used, as well as determining how the data must be secured. Data owners can have legal rights and complete control over information and can authorize or deny access to their data — the data owner is ultimately responsible and accountable for their data. In many organizations, this responsibility may be a senior leader, potentially the chief executive officer (CEO) or a company president, but particular data-related tasks can be delegated. The data owner can be held liable for negligence in the event that sensitive data that they own is misused or disclosed in an unauthorized way; even when day-to-day tasks are delegated, the data owner remains ultimately accountable. The extent of a data owner's responsibility can be established and enforced through legal or regulatory measures. The organization's data policy will outline the manner in which the data owner may use, collect, share, and store data that they own. All data, but especially data with a sensitivity level, must be assigned to an owner.
