
# Secure Coding Practices

Secure coding practices comprise the entirety of actions taken during software and systems development processes. Nonexistent, disorganized, or poorly understood organizational practices tend to produce software and systems with flaws or bugs, which in turn become weaknesses and vulnerabilities. Standardized secure coding practices that are well-documented, disseminated to all stakeholders, and applied effectively throughout the organization can increase the quality of development projects. This leads to software and system development processes that seek to minimize flaws and bugs, as well as testing and monitoring processes designed to catch and fix vulnerabilities as soon as possible. Although a CISSP may not be directly involved with the daily use of secure coding standards, they play a crucial role in defining requirements for such practices and validating organizational practices designed to meet these requirements.

## Standards for Secure Software Development

As with many practices in software security, there are well-regarded standards available that bring together common knowledge and conventions related to secure coding and development. Remember that no standard is a guaranteed match for every organization, so these should be used as starting points from which organizations can develop and implement their own practices, in a process known as *tailoring*.

The OWASP project contains a multitude of resources for secure coding practices. The Cheat Sheet Series (`owasp.org/www-project-cheat-sheets`) is an entirely library of guidance on secure practices, such as the following:
* Implementing authentication and access controls
* Preventing common vulnerabilities and attacks like XSS and injection
* Architectural security for technologies like microservices, REST APIs, and SAML
* Technology-specific security guidance including XML, HTML5, Docker, and Kubernetes

A technology-neutral OWASP Secure Coding Practices Quick Reference Guide (`owasp.org/www-pdf-archive/OWASP_SCP_Quick_Reference_Guide_v2.pdf`) is also available and provides recommendations and best practices across a set of common development activities. Although primarily written for web and mobile apps, the guidance is broadly applicable to almost any application and system architecture.

Each category contains key focus areas for secure coding practices, and these practices can be used to guide the development and implementation of organization-specific practices tailored to your unique technology stack, programming languages, and security requirements. There are 13 categories, each of which contain a checklist of common practices, covering the following:
* Input validation
* Output encoding
* Authentication and password management
* Session management
* Access control
* Cryptographic practices
* Error handling and logging
* Communication security
* System configuration
* Database security
* File management
* Memory management
* General coding practices

The Carnegie Mellon University Software Engineering Institute (CMU SEI) publishes guides covering both secure coding practices as well as secure coding standards. The Top 10 Secure Coding Practices, which actually contains two bonus practices, is a very high-level set of principles for implementing secure coding like using input validation and following the “keep it simple” rule. The Coding Standards are a set of publications designed to provide standards for secure software development in specific languages including C/C++, Android, and Perl. While not entirely focused on security, they do cover best practices for each language and include some security-specific guidance. These documents and others related to secure coding can be found at the CMU SEI CERT Secure Coding wiki at `wiki.sei.cmu.edu/confluence/display/seccode/SEI+CERT+Coding+Standards`.

## Education and Culture

Documentation alone is not enough to solve security problems; the daily activities of all members in the organization must follow the guidance laid out in policies, standards, and procedures. This is the essence of building a security culture, in which all stakeholders understand, support, and actively carry out security efforts, and it is essential that this is widely adopted across the organization, since cybersecurity is a broad discipline underpinning any process that relies on data and information systems. Building security culture obviously encompasses more than just software and system development, but these activities are especially important given their pivotal role in designing secure systems.

Security culture begins with clear and effective policies and other documentation that provides clear guidance to members of the organization on their security roles and responsibilities. Reading policy documents alone is not sufficient, as many security policies will deal with or include concepts that are not familiar to all. While the documentation should be clear enough for broad consumption, additional documentation like procedures, guidelines, standards, and checklists must be created to provide specific guidance to different stakeholders. For example, a policy describing the desired levels of entropy in password construction is no doubt thorough, but the majority of users in the organization are unlikely to know what entropy is. A password section of the acceptable use policy written in plain language, supplemented with training, education, and awareness materials on secure password practices, is a much more suitable way to communicate this requirement. Incorporating security objectives into organizational goals and priorities and promoting engagement with security are also essential.

One crucial element of a security culture is the set of principles it is built on, and the foundational role played by software development means a number of key principles must be considered in order to create a successful security culture. These principles include the following:
* **Secure by design:** This principle largely stems from a paradigm shift as the field of software engineering matured and decisions that led to insecure technologies like DNS and SNMP were examined in light of modern information security risks. Rather than attempting to retrofit security after deployment, this principle espouses practices that require the inclusion of security at the outset of a system: in the design phase. Anticipating that the system will come under attack drives design and implementation decisions that provide inherent security abilities, rather than relying on more expensive and error-prone compensating controls.
* **Secure by default:** This principle is a collection of best practices for delivering more secure software, such as the inclusion of layered security controls and conservative default settings designed for security. Many data breaches share the underlying root cause of improperly secured cloud storage services exposing data to the public. In a secure by default configuration, those services should be private by default and require extra steps be taken to turn on public access.
* **DevSecOps:** Although not strictly a security principle, one of the foundational ideas of DevSecOps is to shift security “to the left” — in other words, to push security activities earlier in the software development lifecycle rather than developing security strategies only after a system is built and deployed. Implementing DevSecOps in an organization is, fundamentally, an opportunity to build a culture of security and deliver higher quality software.
