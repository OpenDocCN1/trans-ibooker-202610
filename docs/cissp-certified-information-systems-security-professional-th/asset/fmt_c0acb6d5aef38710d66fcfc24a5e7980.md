
Assessment and testing can be performed as an internal or external function; while both are appropriate for monitoring security program status, there are situations that require external evaluations. For instance, third-party audits are common in situations where an assessment must be conducted that is free of any conflict of interest. External audit reports, such as the Service Organization Control or SOC 2, can be useful for organizations to communicate details of their security practices to external parties like vendors or business partners. In this case, the auditor's independence from the audited organization provides additional assurance to consumers of the report.

Ethical penetration testing and related technical testing topics are presented in this domain, including test coverage and breach attack simulations. These types of tests can be conducted against a range of targets from individual information systems to entire organizations and are a valuable tool to identify deficiencies in security controls. The disclosure and handling of any findings from such testing is also discussed, including legal and ethical implications of information that might be discovered.

An ongoing assessment and testing program is also useful for establishing continuous monitoring and supporting compliance needs. Properly designed and implemented strategies for testing security controls, vulnerabilities, and attack simulations measure the effectiveness of the organization's existing control program. Any identified deficiencies must be addressed to ensure adequate risk management.

### Security Operations

Security Operations (`SecOps`) is a companion to the other domains in the CBK, and this chapter deals with implementing, operating, and maintaining infrastructure needed to enable the organization's security program. Security practitioners must first perform a risk assessment and then design and operate security controls spanning technology, people, and process to mitigate those risks. `SecOps` is a key integration point between security teams and other parts of the organization such as Human Resources (`HR`) for key tasks like designing job rotations or segregation of duties, or a network engineering team that is responsible for implementing and maintaining firewalls and intrusion detection systems (`IDSs`).

Logical security aspects of `SecOps` include running and maintaining a security operations center (`SOC`), which is becoming an increasingly crucial part of a security program. The `SOC` centralizes information like threat intelligence, incident response, and security alerts, permitting information sharing, more efficient response, and oversight for the security program and functions. Planning for and exercising crucial business plans like business continuity and disaster recovery (`BCDR`) are also an important element of `SecOps`.

`SecOps` also encompasses important physical security concepts like facility design and environmental controls, which are often completely new concepts for security practitioners who have experience in cybersecurity or information technology (`IT`). However, the physical security of information systems and the data they contain is an important element of maintaining all aspects of security. In some cases, physical limitations like existing or shared buildings are drivers for additional logical controls to compensate for potential unauthorized physical access.

### Software Development Security

Information systems rely on software, so proper security is essential for the tools and processes used to develop software. This includes both custom-built software as well as purchased system components that are integrated into information systems. Cloud computing is changing the paradigm of software development, so this domain also includes security requirements for computing resources that are consumed as a service like software as a service (`SaaS`), platform as a service (`PaaS`), and emerging architectures like containerization and microservices.

Software can be both a target for attackers and the attack vector. The increasingly complex software environment makes use of open-source software, prebuilt modules and libraries, and distributed applications to provide greater speed for developers and functionality for users. These business advantages, however, introduce risks like the potential for untrustworthy third-party code to be included in an application or attackers targeting remote access features.

Adequate security in the software development lifecycle (`SDLC`) requires a combined approach addressing people, process, and technology. This domain revisits the critical personnel security concept of training, with a specific focus on developer security training. Well-documented software development methodologies, guidelines, and procedures are essential process controls covered in the domain. Technology controls encompassing both the software development environment and software security testing are presented, as well as testing approaches for application security (`AppSec`) including static and dynamic testing.

# DOMAIN 1

## Security and Risk Management

**DOMAIN 1 OF THE** CISSP Common Body of Knowledge (`CBK`) covers the foundational topics of building and managing a risk-based information security program. This domain covers a wide variety of concepts upon which the remainder of the `CBK` builds.

Before diving into the heart of security and risk management concepts, this chapter begins with coverage of professional ethics and how they apply in the field of information security. Understanding your responsibilities as a security professional is equally as important as knowing how to apply the security concepts. We then move on to topics related to understanding your organization's mission, strategy, goals, and business objectives, and evaluating how to properly satisfy your organization's business needs securely.

Understanding risk management, and how its concepts apply to information security, is one of the most important things you should take away from this chapter. We describe risk management concepts and explain how to apply them within your organization's security program. In addition, understanding relevant legal, regulatory, and compliance requirements is a critical component of every information security program. Domain 1 includes coverage of concepts such as cybercrimes and data breaches, import/export controls, and requirements for conducting various types of investigations.

This chapter introduces the human element of security and includes coverage of methods for educating your organization's employees on key security concepts. We cover the structure of a security awareness program and discuss how to evaluate the effectiveness of your education and training methods.

## UNDERSTAND, ADHERE TO, AND PROMOTE PROFESSIONAL ETHICS

Understanding and following a strict code of ethics should be a top priority for any security professional. As a `CISSP` (or any information security professional who is certified by `(ISC)²`), you are required to understand and fully commit to supporting the `(ISC)²` Code of Ethics. Any `(ISC)²` member who knowingly violates the `(ISC)²` Code of Ethics will be subject to peer review and potential penalties, which may include revocation of the member's `(ISC)²` certification(s).

### (ISC)² Code of Professional Ethics

The `(ISC)²` Code of Ethics Preamble is as follows:

* The safety and welfare of society and the common good, duty to our principals, and to each other, requires that we adhere, and be seen to adhere, to the highest ethical standards of behavior.
* Therefore, strict adherence to this Code of Ethics is a condition of certification.
