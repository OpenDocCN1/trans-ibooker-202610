
- **Memorandum of Agreement (MOA):** An MOA is similar to an MOU but serves as a legal document and describes the terms and details of the agreement.
- **Non-Disclosure Agreement (NDA):** An NDA is a legally binding contract made between an employee or a business partner where they promise not to disclose trade secrets to others without proper authorization. The reason for this is to stop trade secrets or proprietary information from being sold on to competitors.
- **Service Level Agreement (SLA):** An SLA is a contract between a service provider and a company receiving the service. The agreement can be for either a fix or a response over a certain period of time and is measured by metrics.
- **Measurement Systems Analysis (MSA):** MSA is a process wherein systems are evaluated by the collection of statistics to ensure that the quality of the systems is effective.
- **End of Life (EOL):** This is where a vendor stops selling a product and the availability of replacement parts and technical support is limited. Warranties are still honored.
- **End of Service Life (EOSL):** This is where the vendor believes that the product has reached the end of its usefulness, maybe due to a new version of the product being released. They will not commit any more time or resources to maintain the product. Users can still use the product but must take into consideration that there will be no more security updates or technical support available from the vendor. This will mean that over time, such products will become vulnerable and pose a huge security risk to any company still using them.

> **Exam Tip**
> 
> An SLA lays down how quickly a supplier should respond to an incident such as a failed printer. It is measured using metrics.

## Data

Data is one of the most important assets that a company has, and it is important to ensure that policies are in place to ensure that it is classified, handled, stored, and disposed of in accordance with regulations such as GDPR or HIPAA. Let's look at the data aspects we need to consider, starting with classification:

- **Classification:** This is the process of labeling data with relevant classifications, so that we know if it is top secret, secret, confidential, or sensitive data. The classification determines how the data is handled.
- **Governance**: Data governance is the oversight and management that describes the security controls that are applied at each stage of the data-handling process, from creation to destruction. These procedures detail the processes used to manage, store, and dispose of data to ensure that you are compliant.
- **Retention**: Companies do not want to hold data any longer than they need to, as it reduces their liability; however, they may have to keep data in an archive after its usefulness to remain compliant. An example of this is medical data in the UK, which needs to be retained for 25 years.

## Credential Policies

Credentials must be kept safe to prevent unauthorized access to systems; therefore, it is vital that policies are in place to prevent vulnerabilities and unauthorized access. Let's look at the policies we need to put in place, starting with personnel-related policies:

- **Personnel**: Personnel accounts could use a shared account, where all members of the customer service team use the same account to email customers. The downside of this is that you cannot audit or monitor individual users. The other type of personnel account is the user account, which should be subjected to the principle of least privilege.
- **Third-Party**: A third-party credential could be a SAML token given by a cloud provider or a **Security as a Service (SECaaS)** vendor, where the cloud provider manages your identity management. If we are doing remote administration, we could use SSH keys for Secure Shell, where the public key is installed on the target server.
- **Devices**: Devices have generic accounts with default password settings. As soon as you purchase a device, you need to change the default settings, as these are published on websites. Please go to https://cirt.net/passwords to see these passwords.
- **Service Accounts**: Service accounts are used to run applications such as anti-virus. They can run as local service accounts with the same rights as a user. A system account gives you a higher level of privilege, giving you full control.
- **Administrator/Root Accounts**: Administrative and root accounts in Linux need to be protected as they allow you to install software, make configuration changes, and access any file. These accounts should be restricted to a few IT personnel. When we install new systems, we need to ensure that we change the default account settings. The root account in Linux is called *superuser* and is not restricted; this account should not be used unless it is absolutely necessary. The administrator should have two accounts: a normal user account for day-to-day use and an admin account for administrative duties.

## Organizational Policies

Organizational policies need to be in place to deal with changes in technology, risk, or security to maintain a secure working environment. Let's look at some of these policies, starting with change management:

- **Change Management**: When an audit is carried out and reports show that the controls in place are not secure enough, we either implement change management or write a new policy. A new policy changes an entire process, and change management amends existing processes.
- **Change Control**: Change control is where someone requests those managing the implementation of a change to an existing control. Let's say that management would like to know about the financial benefits of a change, and the savings that would be gained either in labor time or monetary value. Such changes need to be sent to the **Change Advisory Board (CAB)** to ensure that it is beneficial to the company.
- **Asset Management**: This is a process where each asset that belongs to the company has been tagged and is recorded in an asset register. Annual audits need to be carried out to ensure that all assets are accounted for. It will also help to identify unauthorized devices on your network.

## Regulations, Standards, and Legislation

Regulations, standards, and legislation are put in place to ensure that compliance has been achieved, and most are legally enforceable. From these regulations and standards, we derive our policies to ensure compliance and prevent crime; if companies do not abide by these regulations, they will be fined. Other industry frameworks are only best practices and are not legally enforceable, but vendors will not support any product that has not been set up according to such best practices. In this section, we are going to look at regulations, standards, and legislation followed by a look at key frameworks, benchmarks, and secure configuration guides. Let's start with **General Data Protection Regulation (GDPR)**:

- **General Data Protection Regulation (GDPR):** The European Union's (EU's) GDPR came into force on 25th May 2018, as a framework for data protection law. It is enforced by the EU **Information Commissioner's Office (ICO)** and protects the individual's right to the privacy of their data, such as name, date of birth, photographs, video footage, email addresses, and telephone numbers. GDPR aims at protecting the collection, use, and storage of this information. GDPR states that anyone selling products on their website to EU citizens must adhere to GDPR and that data can only be stored for its intended use. For example, after a purchase has been made, credit card details should no longer be stored without the consent of the user. It is known as an international regulation as there are 27 countries in the EU.
- **National, Territory, or State Laws:** In the US, there are national data laws – for example, the **Health Insurance Portability and Accountability Act (HIPAA)** for medical data and the **Gramm-Leach-Bliley Act (GLBA)** for financial services. States tend to have their own laws on personal data, but most are based on **Federal Information Security Management Act (FISMA)**, which protects government information and operations.
- **Payment Card Industry Data Security Standard (PCI DSS):** PCI DSS deals with the handling and storage of data used for card payments. For more information, please go to `https://www.pcisecuritystandards.org/about_us/`.

> **Exam Tip**
> 
> GDPR deals with the handling of data while maintaining the privacy and rights of an individual. It is said to be international as it was created by the EU, which has 27 different countries as members.

## Key Frameworks

Several key frameworks have been designed, mostly by not-for-profit organizations, to help reduce the risk created by ever-increasing cybercrime levels and the adoption of the cloud by companies. Let's start with **Center for Internet Security (CIS):**

- **CIS:** This is a not-for-profit organization that publishes information on cybersecurity best practices and threats and has tools to help harden your environment and provide risk management. CIS provides benchmarks for different operating systems and provides controls to help secure your organization. For more details, go to `https://www.cisecurity.org/cybersecurity-tools/`.
