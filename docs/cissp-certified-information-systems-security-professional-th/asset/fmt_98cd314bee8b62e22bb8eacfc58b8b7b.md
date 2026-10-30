
There is a subtle but important difference between *events* and *incidents*; both are important for continuous monitoring and the practice of incident management.

*   *Events* are any observable item like routine user or system actions, such as a user successfully logging into a system or a file being accessed. Many routine events will be logged but do not require additional action.
*   *Incidents* are events that are both unplanned and have an adverse impact on the organization. They typically require investigation and remediation by some combination of IT, operations, and security personnel. Examples of incidents include unexpected restart of a system, ransomware preventing users from accessing a system, or a planned system outage that goes beyond the allotted time.

All incidents should be investigated and remediated to restore the organization's normal operations as quickly as possible and to minimize impacts like lost productivity or revenue. Resuming normal service is the primary goal of incident management. While legacy incident management processes focused on manual detection, investigation, and response to incidents, SOAR platforms are designed to reduce the time required to manage incidents. They do so by gathering and analyzing data to detect incidents and then automating response actions needed to defend and protect systems.

Not all incidents will require participation by the security team. For example, increased system utilization due to a new product launch is expected and simply requires operations resources to add additional capacity. A coordinated DoS attack by a foreign nation state, however, would require participation by both IT and security personnel to successfully remediate.

## Incident Management Plan

Many security frameworks refer to incident handing as *incident response*, while operational frameworks such as ITIL use *incident management*. The goal of both is largely the same—dealing with the incident and restoring the organization to normal operations. The foundation of this IR capability is a documented IR plan, which is a proactive control designed to help the organization respond more effectively when an incident occurs.

The IR plan should document the tools, resources, and processes needed to identify, categorize, and remediate the impact of incidents. The IR plan typically contains the following:

*   Definitions of incident types, such as internal operational incidents, security incidents, or emergency situations that require first responders.
*   The incident response team (IR team) personnel. The team members and skills needed will depend on the type of incident, but an incident response coordinator should always be appointed to assess the situation and identify the skills and actions needed.
*   Roles and responsibilities for the IR team personnel in each incident type. This includes roles internal to the organization, as well as responsibilities of external stakeholders like law enforcement or business partners.
*   Resources required, such as operations or security management tools to facilitate detection and response like the SIEM or IDS. Checklists and procedure documentation is useful during an emergency to ensure team members perform the correct functions during a stressful event.
*   Incident management processes must be laid out according to the lifecycle phases presented in this section, including processes for personnel to follow when an incident is first detected, under triage, and once a remediation plan is determined. Incidents must be detected to begin the process, and then the IRT will execute appropriate steps to investigate, respond, and remediate.

All incident response frameworks emphasize the importance of planning ahead for incidents by identifying likely scenarios and developing appropriate response strategies for each before they occur. Incidents can be a high-stress situation, and *ad hoc* responses are less preferable than preplanned, rehearsed responses. A variety of standards exist to support organizations developing an incident response capability, as highlighted by the following list:

*   ITIL framework incident management processes
*   NIST Special Publication 800-61, “Computer Security Incident Handling Guide”
*   ISO 27035, “Security incident management”
*   European Network and Information Security Agency (ENISA), “CSIRT Setting Up Guide”
*   ISACA, “Incident Management and Response”

Another important aspect of the organization's incident management capability is the proper categorization and prioritization of incidents based on their impact and criticality. This is similar to risk assessments where risks are analyzed according to their potential impact and likelihood. Incidents have already occurred, so they are often measured by:

*   **Criticality/impact**: The effect the incident has on operations
*   **Urgency**: The timeframe in which the incident must be resolved to avoid unacceptable impacts

Many organizations utilize a priority score from zero to five (P0–P5) to categorize incidents. P0 is the most critical, and P5 is the least critical, and members of the IRT use this score to prioritize the work they must perform. In many organizations, an incident at or above a certain priority rating may also be a trigger for other organizational capabilities, such as invoking BC or DR plans.

### Incident Response Testing and Exercise

IR planning is a proactive control, so the IR plan must be documented before an incident occurs. Conducting tests and exercises of the IR plan serves two vital functions. First, testing the plan in a nonemergency situation can help to identify gaps, oversights, or issues that could prevent the successful execution of the plan during a real incident. Second, the exercise can be used to train members of the IR team on their duties, which reduces confusion or wasted effort during a real incident because team members are able to respond from memory.

IR testing can be conducted to exercise a particular scenario, with the goal of identifying how effective the plan is in dealing with that type of incident. For example, widespread outbreak of ransomware could be used as a scenario to gauge how effectively backup procedures or systems are at taking over operations. IR exercises can be integrated with the testing or exercise of other plans like BC and DR, as security incidents often create situations that necessitate the activation of these other processes.

### Third-Party Considerations

Most organizations will have some dependencies on external entities including suppliers or vendors, especially for services like utilities and cloud computing. Activities like IR and business continuity and disaster recovery (BCDR) may need to include these vendors to ensure proper coordination with external service providers to allow the organization to continue functioning. The IR plan should identify any key external service providers or third parties like digital forensics and incident response (DFIR) providers, cyber insurance carriers, legal or communications experts, and data breach specialists who might need to be involved with an incident response. The IR plan should also contain contact information for these third parties to ensure the organization can engage any services needed during an incident.

Third parties whose services need to be utilized in emergencies should be included in the IR plan. Each provider's contact information should be documented for easy reference, as well as the circumstances that require the provider to be involved. Roles and responsibilities of the third party should also be documented, so it is clear to the team which tasks must be performed internally and which tasks will be handled by the third party.

## Detection
