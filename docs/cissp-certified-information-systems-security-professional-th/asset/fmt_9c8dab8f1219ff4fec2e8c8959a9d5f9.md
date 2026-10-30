
Many of the considerations with third-party connectivity align with the topics covered in the “Remote Access” section of this chapter and throughout the rest of this book. You should follow best practices, such as encryption in transit, strong access control procedures, and strong email filtering. In addition, continuous monitoring of any third-party connections should be standard practice to identify new threats, unexpected behavior, or deviations from agreed-upon SLAs.

A major category of risk involved with third-party connectivity is compliance risk. As the owner of your and your customers' data, your organization is responsible for understanding how the external parties manage and secure your data. You must confirm that their controls meet your legal and regulatory obligations to protect the data, including (but not limited to) considerations around the location of the servers and systems that host and process the data. Compliance reports, such as `SOC2` reports, can serve as a good starting point in assessing security and compliance risks associated with third-party connectivity, and SLAs are again pivotal in holding third parties accountable.

## SUMMARY

Communication and network security used to be restricted to protecting assets and information within the physical confines of an organization. Today, the boundaries of a corporation have changed dramatically. Companies might have workers at home, in coffee shops, or in hotel lobbies. The boundaries of a company extend far beyond its physical buildings, so the structures, transmission methods, transport formats, and security methods used to provide confidentiality, integrity, and availability for transmissions over public and private communications networks and media have changed to accommodate those extended boundaries.

This chapter contains fundamental concepts such as telephony, data communications, the different layers of the OSI model, and transmission media and protocols. Understanding the historical development of communications technology interfaces, interoperability, and high-level approaches to leverage security controls to make everything work together and safeguard information is invaluable for network security professionals.

Security professionals need to develop skills in improving and administering remote access, virtualization, and cloud services to enable new business and workplace models. Along with these information technology changes come security challenges and opportunities. One of the foundational concepts that continue to underscore proper security management is that no single solution satisfies all gaps. A security professional has to understand the layered solution approach that describes defense in depth. Access control, authentication, and encryption, for example, work together to extend corporate computing borders, allow employees to work from anywhere, and reduce costs of providing information security. The nature of security attacks will also continue to evolve. In this chapter we discussed several types of attacks, like `DDoS`, `MITM`, and phishing, to underscore the importance and the dynamic nature of information security as a profession. As the threats change, so must our approaches to thwarting them.

# DOMAIN 5

## Identity and Access Management

`IDENTITY AND ACCESS MANAGEMENT` (`IAM` or `IDAM`) is fundamental to information security. Controlling access to resources requires the ability to identify and validate the entities requesting access and to hold them accountable for the actions they take. Entities can be users, systems, applications, or processes, and IAM consists of four foundational elements: identification, authentication, authorization, and accountability (`IAAA`).

This chapter identifies each of the `IAAA` elements and how they can be configured and enforced, which forms the basis of many access control systems. Controlling physical and logical access to assets typically starts with establishing identities and providing users with credentials, which are then presented and authenticated whenever access is requested. From time to time, these credentials must be reviewed and updated, and at the end of their useful life, they are decommissioned. Creating and retiring access credentials is known as *provisioning* and *deprovisioning*, and the process for managing credentials can be viewed as a lifecycle.

IAM is involved in practically every aspect of an organization's security program, so a Certified Information System Security Professional (`CISSP`) is well served by ensuring adequate procedures and technical systems exist to support access control arrangements. This starts with understanding the requirements for identity information, choosing and implementing systems that meet those requirements, and ensuring administrative controls like access reviews support the technology controls to preserve security in systems.

## CONTROL PHYSICAL AND LOGICAL ACCESS TO ASSETS

Physical access controls should be familiar to virtually all readers — doors, curtains, fences, etc., are common everyday objects used to control physical access to buildings like homes, schools, and office buildings. Logical access controls are similarly omnipresent, though they are not always as obvious to the untrained eye. Systems that require a password, personal identification number (`PIN`), or shared symmetric key for access are all examples of logical access controls. Both logical and physical access measures are designed to restrict which users or systems can access specific assets like data in a system or valuables stored in a safe.

There are a variety of access control methods, and choosing the correct combination requires a security practitioner to understand the organization's assets and priorities for controlling access. For example, valuable physical assets and a requirement for strong confidentiality demands strong physical access controls, while information systems with strict requirements for data integrity may require technical controls like application logic to meet requirements.

Access control systems typically enforce controls over subjects accessing objects. A *subject* can be a user or program, and an *object* is whatever the subject is trying to access, like a file, application, facility, or database. Controlling access via these systems is crucial for enforcing security concepts like confidentiality, integrity, and nonrepudiation.

### Access Control Definitions

Access control models use a standard set of terms to discuss various concepts. These include the following, which are used throughout the remainder of this chapter:

*   **Objects** are assets that require access control. Common objects include information like files or datasets as well as system objects like compute resources and networks. Physical access control models deal with facilities as objects, such as office buildings, computer rooms, and sensitive information handling facilities. All objects share a common need for protection of basic security attributes including confidentiality, integrity, availability, nonrepudiation, and authenticity (`CIANA`).

*   **Subjects** can be human users or nonhuman entities like systems, processes, and devices. Subjects require access to objects like files, systems, and facilities, and the fundamental practice of access control defines how such access is granted, used, and monitored.

*   **Access** is anything a subject is permitted to do with or to an object. For example, a human user can be allowed or denied entry to a facility, a system user can be allowed read access but denied write and update access to a file, and nonhuman users like system processes can be allowed or denied access to specific data objects based on the sensitivity level of the objects in question. Important concepts related to controlling access including authorization are discussed throughout this chapter.

### Information
