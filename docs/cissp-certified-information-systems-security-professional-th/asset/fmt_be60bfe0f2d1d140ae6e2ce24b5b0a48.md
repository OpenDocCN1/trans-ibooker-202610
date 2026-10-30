
Various audit and assessment methodology standards exist and may be used as a starting point to guide the organization's strategy. Utilizing a standard methodology offers key benefits including repeatable testing and results that will be more comparable over time. Auditing against the same standard year over year can demonstrate return on investment (`ROI`) from the organization's spending on security efforts, as well as the increasing maturity of key security practices or processes. All concepts introduced here are revisited in more detail later in this chapter in the section “Conduct or Facilitate Security Audits.”

## Internal

Audits, assessments, and testing can be conducted from two perspectives. Internal evaluations are done inside the organization, with the staff and resources of the organization. Any use of third-party assessors or auditors constitutes an external perspective, which is discussed in the next section. When designing an audit strategy, different circumstances will drive the choice of internal, external, or even the use of both audit perspectives.

To conduct an internal audit, the organization must have adequate skills and knowledge on staff, it must have sufficient time and resources to perform the audit, and there must be no requirements for the audit or assessment to be performed by an independent observer who is totally free of conflicts. Assessments conducted from an internal perspective are appropriate as part of ongoing or continual assessment processes and are often used as part of continuous monitoring with a supplemental external audit performed less frequently.

Depending on the size of the organization, there may need to be a full-time staff member dedicated to assessment activities, or elements of the assessment may be distributed among multiple staff members. For example, access control reviews are often conducted once per quarter and may require only a few hours of work, so a single IT staff member can perform them in addition to other work assignments. In a large multinational organization with complex user access requirements, multiple offices, and hundreds or thousands of systems, access control reviews could be a full-time job.

There is a general saying that “Whoever wrote the policy shouldn't be the one to audit it,” and mitigating conflicts of interest is an important element in an audit or assessment strategy. An internal auditor may be unintentionally blind to faults or may intentionally cover mistakes or deficiencies in the interest of maintaining their position. In some cases, this conflict of interest may be mitigated by using an internal audit team that does not report directly to any of the department heads being audited.

While conflict of interest can be an issue for internal audits, greater knowledge of skills, process, and systems in use at the organization is a potential positive. Internal testers can more accurately perform tests or assessments with the knowledge of an insider, which helps identify issues related to insider threat. Performing testing on complex systems with complex rules like separation of duties (`SoD`) can be challenging for an outsider.

These are some types of testing that may be best conducted internally:
* Vulnerability scanning, especially routine scans looking for unpatched software or unknown assets
* Process and procedure audits like change management or training completion
* Phishing simulations

## External

The other perspective for conducing audits, assessments, and testing is external to the organization. An external auditor should have no conflicts of interest, since they are paid to perform an impartial assessment rather than to deliver a report that makes their department look good. External evaluations can also take advantage of specialized skills and knowledge that are valuable but not needed full-time. Performing penetration testing, often called pen testing, is an important and valuable skill, but due to the specialized skills and high salaries required, employing a full-time pen tester is not cost-effective for most organizations. Contracting an outside party to perform the test and sharing the costs with other organizations is a more sensible approach.

The use of an external firm for assessments, testing, or audits can identify issues the organization is unintentionally unaware of; a fresh pair of eyes can often spot a problem more quickly and offer insight the organization would otherwise miss. External testing may be advisable or even required in some cases, especially for legal or regulatory reasons, such as maturity assessments against a maturity model, which is covered in more detail in [Chapter 8](#c08.xhtml), “Software Development Security.”

Audits for compliance purposes — for example, `PCI-DSS`, `ISO 27001`, and the U.S. government's Federal Risk and Authorization Management Program (`FedRAMP`) — all require the use of an independent, authorized third-party auditor or assessor. The results of these assessments also need to be reliable for any organization doing business with the audited firm, so the use of a third party with no conflict of interest increases the assurance provided by the audit.

## Third-Party

Most organizations rely on a complex series of interconnected organizations, known as a *supply chain*, to support their operations. A standard office building with workers performing basic data processing and collaboration relies on hardware manufacturers, utility providers, and probably cloud services, just to name a few. Vulnerabilities in this supply chain can impact the organization, so an audit strategy is needed to identify risks and associated mitigations in the supply chain.

Supply chain audits are a topic introduced in [Chapter 1](#c01.xhtml), “Security and Risk Management,” and are a vital part of any security risk management program. A sound third party audit strategy must be scoped to include any third-party critical to the organization's operations, handling any data valuable to the organization, or providing regulated services like payment processing. The audits may be performed by organizational resources or a third-party auditor retained by the organization to audit their suppliers. Some suppliers may be able to provide a report performed by a trusted third party, such as a Service Organization Control 2 (`SOC 2`) Type II report, which provides details about the supplier's security controls and their effectiveness. A `CISSP` must verify if the services their organization is using were in scope of the audit report provided by a supplier.

# CONDUCT SECURITY CONTROL TESTING

Testing controls is essential to ensure they are implemented and operating as intended to mitigate risks. Over time, processes and systems change, and the risks facing the organization evolve, so a `CISSP` must design a control testing program to provide reliable and accurate data about the state of controls. This is a crucial piece of oversight that must support the ability to take action if a control is found to be deficient or no longer effective.

Testing may be done from internal or external perspectives and may target complex control implementations across people, process, and technology. For example, testing a media disposal control might entail policy and procedure reviews, staff interviews, examination of disposal records, and physical selection of disposed hard drives or other media to test by plugging them into a computer to see if data is recoverable.

## Vulnerability Assessment
