+   - Fire rules and load balancers
+   - Cloud Security Scanner
+   - Monitoring and logging
+   - Encryption
+   - Penetration testing in GCP
+   - Industry regulations
+   - CI/CD security overview
+   - Additional security services

# Introduction to security

Let's start this chapter with a brief introduction to GCP's approach to security. As we mentioned previously, security is not an afterthought and is built into its services. But before we even think about securing our services, we need to acknowledge the fact that GCP has a holistic view of security. This can be seen by restricting physical data center access and using custom hardware and hardened versions of operating systems in the software stack.

> **Important Note**
> Google uses custom hardware with security in mind and uses a hardened version of Linux for the software stack, which is monitored for binary modifications and enforces trusted server boots.

Storage is a key service for any cloud provider, and GCP offers encryption at rest by default on all storage services. This can support customer encryption keys or manage keys on behalf of the customer. On physical storage disks, retired disks will have sectors zeroed and if data cannot be deleted, disks are destroyed in a multi-stage crusher. However, note that if a customer deletes data, it can take 180 days to be physically deleted.

Additionally, one of the main security concerns is attacks from outside organizations. To protect us from internet attacks, Google offers the ability to register against the frontend, which will check incoming network connections for correct certificates, and offers protection against **Distributed Denial-of-Service (DDoS)** attacks. In addition, using GCP load balancers will offer extra protection, while cloud VPNs and direct connections offer more encryption options.