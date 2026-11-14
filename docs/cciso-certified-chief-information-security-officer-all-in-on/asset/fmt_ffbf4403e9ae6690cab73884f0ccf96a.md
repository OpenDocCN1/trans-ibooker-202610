
Security incident handling begins with an indicator of a potential security incident and ends with resolution and closing. There are many steps along the way, and each step must be well planned, guided by policies, and performed following well-crafted and rehearsed procedures. As with all aspects of security, the *extent to which* the organization plans, rehearses, and documents incident handling procedures is based on the nature of the business and its tolerance for risk.

## Involving Law Enforcement

During the course of an investigation, the organization may identify suspect actors. These actors may be external individuals or internal employees of the organization. In either case, if there is a suspicion that a crime may have been committed, law enforcement should be called in. However, the decision of when and how law enforcement is contacted should not be taken lightly as there may be implications for the organization. There should be procedures in place to provide guidance regarding when law enforcement agencies should be contacted (the reporting threshold), who should be involved in the decision, and how such contact should be carried out and by whom.

The organization should involve legal counsel in the formulation of incident handling procedures, especially those procedures that address suspected criminal activity and interacting with law enforcement.

When law enforcement is contacted, the organization relinquishes control of the investigation to the law enforcement agency. As a consequence, the law enforcement agency may take control of, or possession of, the organization’s asset(s) to conduct their investigation. This may cause a disruption of the organization’s business operations and is one, sometimes overlooked, reason to have good BCP/DR processes in place so that the organization can restore business operations using backup or alternate systems and data.

Certain information must immediately be reported to law enforcement agencies as required by law, such as if certain kinds of contraband is discovered on a company computer. However, some incidents are reported at the organization’s discretion, such as employee violations of access control policies which technically may violate the law but are often unreported if no sensitive data (such as HIPAA PHI) is illegally accessed.

## Employee Policy Violations

Security policies are no different than HR policies in that employees must be held accountable for compliance. The obligation of the organization is to ensure policies are enforced consistently. If an organization does not enforce policies consistently, it leaves itself open to employees filing claims against it. Rules for good security policy enforcement include the following:

- Publish policies and ensure all employees are made aware of them.
- Define and document processes for reporting security violations, investigating them, and carrying out enforcement. Enforcement may include disciplinary actions up to and including employee termination.
- Provide training to managers and supervisors not only in the policies themselves but in reporting and enforcement.
- Include security policy coverage in new hire orientations and periodic refresher briefings.

Every organization should have a mechanism in place for employees to report suspected violations of security policies. Employees should be made to feel safe from any repercussions of reporting what they see. Some organizations accomplish this by having an anonymous reporting option, a whistleblower program, or a designated ombudsman that serves as a proxy between the person reporting the violation and the organization.

## Incident Handling Process

As introduced in [Chapter 1](#ch1.xhtml), incident handling is commonly implemented using a process similar to that depicted in [Figure 4-20](#ch4.xhtml#fig4_20).

![Figure 4-20](Images/F04-20.jpg)

**Figure 4-20** Six steps of incident handling

The figure shows the current industry practice for handling incidents. Although the exact names and number of steps vary from organization to organization (NIST defines a four-step process), the underlying concepts are universal: prepare for incidents ahead of time, investigate and resolve them, restore the organization to normal operations, and then see what can be learned from what happened so incidents like it can be prevented or handled better in the future. Each step from the diagram is discussed in the following sections.

### Prepare

The preparation phase involves addressing the following prerequisites for a good incident handling program:

- Provide oversight of incident handling activities, to ensure that each business unit or group within the organization conducts appropriate incident handling processes.
- Produce, publish, and maintain an incident handling plan, with supporting policies, guidelines, and procedures, that covers the entire organization and enterprise.
- Put a training program in place to ensure that personnel are adequately trained in incident handling methods.
- Define roles and responsibilities for all personnel involved in incident handling and ensure that the roles and responsibilities align with the incident handling plan, policies, and procedures.
- Acquire and deploy adequate tools to support incident handling activities.
- Prepare specific procedures to handle specific types of incidents.

### Identify

Once an incident occurs, the security team must identify what is happening. This starts when a team member sees evidence of an incident. They may begin to see alerts from an IDS or SIEM, indications of anomalous activity in log files, unusual network activity, reports from employees, and so on. At some point the evidence (or accumulation of evidence) reveals that a security incident may have occurred (or is in the process of occurring) and it is time to begin containment.

### Contain

Once an incident is identified, the team must contain it so that it can be further assessed while preventing it from spreading or causing additional damage. Containment strategies should be defined in advance so (ideally) a specific strategy can be invoked for a given type of incident. Containment strategies may include:

- **Perimeter containment** Actions to prevent the spread beyond the network perimeter, such as inbound/outbound traffic blocking, updating perimeter firewall policies, and switching to alternate communication links.
- **Internal containment** Actions to prevent the spread beyond a portion of the network, such as isolating segments or VLANs and updating router, firewall, and switch settings to block specific IP addresses or ports.
- **Endpoint containment** Actions to prevent the spread beyond a device, server, workstation or laptop, such as disconnecting network access for specific endpoints.

### Eradicate

This step could also be called “fix it.” The security team removes from the affected system(s) the malware, program, settings, or accounts causing the issue. Ideally, the team takes care to minimize the loss of data, but this is not always possible. Eradication is often combined with the recover phase, such as re-imaging the disk, which wipes out the bad content and replaces it with good content all in one step.

### Recover

Recovery involves bringing the system(s) back to an operational state. Recovery may include re-imaging the disk, installing new operating systems and applications, updating the STIGs, or hardening settings and installing patches. Recovery brings the system to a new clean and fully functional operating state.
