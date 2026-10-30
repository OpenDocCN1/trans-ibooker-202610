
## Section 3:
Secure, Manage and Monitor a Google Cloud Solution

In this section we will focus on securing your environment, how to use the various tools to manage GCP, and also how we monitor service operations.

This part of the book comprises the following chapters:

- Chapter 15, Security and Compliance
- Chapter 16, Google Cloud Management Options
- Chapter 17, Monitoring Your Infrastructure

## 15
Security and Compliance

Security in GCP was built in from the start and certainly not an afterthought! In each service that GCP provides, you will see that security is paramount. In the public cloud era, it’s true that a single breach of security can not only cause direct financial implications but can also have a detrimental effect on future business for a company. For this reason, GCP takes the view that the best way is in-depth defense rather than a single piece of technology, and it offers a lot more security than what a customer can usually afford by replicating on-premises. In this chapter, we will look at the key topics that we need to understand to be successful in the exam and try to give an overview of security and the importance GCP places on it.

In this chapter, we will cover the following topics:

- Introduction to security
- Cloud Identity
- Resource Manager
- IAM
- Organization Policies
- Service accounts
- Firewall rules and load balancers
- Cloud Security Scanner
- Monitoring and logging
- Encryption
- Penetration testing in GCP
- Industry regulations
- CI/CD security overview
- Additional security services

### Introduction to security

Let's start this chapter with a brief introduction to GCP's approach to security. As we mentioned previously, security is not an afterthought and is built into its services. But before we even think about securing our services, we need to acknowledge the fact that GCP has a holistic view of security. This can be seen by restricting physical data center access and using custom hardware and hardened versions of operating systems in the software stack.

> **Important Note**
> Google uses custom hardware with security in mind and uses a hardened version of Linux for the software stack, which is monitored for binary modifications and enforces trusted server boots.

Storage is a key service for any cloud provider, and GCP offers encryption at rest by default on all storage services. This can support customer encryption keys or manage keys on behalf of the customer. On physical storage disks, retired disks will have sectors zeroed and if data cannot be deleted, disks are destroyed in a multi-stage crusher. However, note that if a customer deletes data, it can take 180 days to be physically deleted.

Additionally, one of the main security concerns is attacks from outside organizations. To protect us from internet attacks, Google offers the ability to register against the frontend, which will check incoming network connections for correct certificates, and offers protection against **Distributed Denial-of-Service (DDoS)** attacks. In addition, using GCP load balancers will offer extra protection, while cloud VPNs and direct connections offer more encryption options.

Google invests heavily in securing its infrastructure, which is designed in progressive security layers. As a quick reference, let's briefly look at these main infrastructure security layers:

- **Securing Low-Level Infrastructure:** Google incorporates multiple layers of physical protection. As they design and build their data centers, access is limited to a very small group of Google employees. Biometric identification, metal detection, vehicle barriers, and laser-based intrusion detection systems are just some of the technologies used to provide a physical layer of protection.

In addition to this, Google also designs server boards and networking equipment, allowing Google to vet the vendors they work with and perform audits and validation on security properties provided by the components. Custom hardware security chips allow Google to securely identify and authenticate Google devices at a hardware level.

Finally, Google uses cryptographic signatures over low-level components such as the kernel, BIOS, and base operating system to ensure server machines are booting the correct software stack.
- **Securing Service Deployment:** A service is an application binary that a developer has written and wants to run on Google's infrastructure. Each service that runs on the Google infrastructure will have an associated service account identity and is used by the clients to ensure communication with the intended server. Google uses cryptographic authentication and authorization for inter-service communication at the application layer.
- **Securing Data Storage:** Google's various storage services can be configured to use keys from a central key management system to encrypt data before it is written to physical storage. This allows Google's infrastructure to isolate itself from potential threats such as malicious disk firmware. Hardware encryption support in the hard drives and SSDs that are used can also track each drive through its life cycle.
- **Securing Internet Communications:** Google's infrastructure consists of large sets of physical machines, connected over LANs and WANs, so there is a need to defend against DoS attacks. This is helped by only exposing a subset of machines directly to external internet traffic. There are also services such as Cloud Armor, which assist in preventing DoS attacks.

Additionally, things such as multi-factor authentication assist in securing internet communication.
- **Operational Security:** Google also needs to operate the infrastructure securely. Google has code libraries and frameworks that will eliminate XSS vulnerabilities in web apps, as well as tools to automatically detect security bugs. They also offer a Vulnerability Reward Program that pays anyone who can discover bugs and is heavily invested in intrusion detection.

> **Exam Tip**
> We will talk about security in more depth than what is expected for you to be successful in the exam; however, it is vital to understand this information for the role of a cloud architect. For the exam, there are several areas that we recommend that you focus on: understanding Cloud IAM roles, understanding what Cloud Identity is, understanding what **Key Management System (KMS)** does, understanding the difference between **Customer-Supplied Encryption Keys (CSEKs)** and **Customer-Managed Encryption Keys (CMEKs)**, being aware of encryption at rest and in transit, and being aware of PCI compliance.

### Shared responsibility model

Before we look deeper into security options, it is important to point out that like the other main public cloud vendors, GCP has what is known as a shared responsibility agreement. As we touched on previously, Google invests a lot in the security of its infrastructure. However, we, as consumers of the platform, also have responsibilities.

Google is not responsible for what resides in the operating system. It's important to understand the principle of shared responsibility for the exam. Google won't be responsible for everything in your architecture, so we must be prepared to take our share of the responsibility. For the exam, we should understand that the different GCP services that are offered mean that Google and the customer will have different levels of responsibility. As an example, let's look at Cloud Storage. Google will manage the encryption of storage and the hardware providing the service and will allow audit logging. However, it will not be responsible for the content that resides on Cloud Storage or the network security that's used to access it. Likewise, access to and authentication of the content will be the owner's responsibility. Compare this to, let's say, BigQuery, which is a PaaS offering. In this case, Google would take responsibility for network security and authentication.
