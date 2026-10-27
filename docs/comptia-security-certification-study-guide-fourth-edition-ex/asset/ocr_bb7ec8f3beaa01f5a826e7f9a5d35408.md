# Credential Policies

Another type of policy that should exist is credential policies. Credential policies determine the credentials, or usernames, used by different entities within the business. The following are some common considerations for credentials:

+   - **Personnel** Each employee within the company should have their own credentials for accessing the network and computer systems. Employees should be taught security best practices with credentials, such as not sharing credentials with other employees.
+   - **Third party** Third-party entities such as business partners or contractors should also have credentials to access systems that are needed. It is important to follow best practices with these credentials, such as ensuring that account expiration options are set on contractors’ credentials.
+   - **Devices** You may need to manage credentials for devices in a few different ways. First, you would set passwords on devices so that only authorized persons can access those devices. You should also set the idle timeout on each device so that the device automatically locks after a short idle time.
+   - **Service accounts** Software that runs on a system also needs to run as “someone,” so you should create what is known as a service account for the software. After creating the account, you then configure the software to use that account to authenticate to the system. This becomes the security identity of the software as it accesses resources on the network.
+   - **Administrator/root accounts** You should also manage the credentials of administrator accounts on systems (also known as root accounts). It is important to ensure that the administrator account on each system is using a different password so that if the password is compromised on one system, the same password cannot be used on the other systems.

![](bbox=[0.073, 0.644, 0.887, 0.786])

**For the Security+ certification exam, be familiar with security best practices in regard to credential policies such as configuring software to use service accounts and ensuring the administrator account on each system is configured with a different password.**

# Privacy and Sensitive Data Concepts

Understanding the security risk to the company and its data is an important skill for security professionals. In this section you will learn about the consequences to a company if it has a security breach, the types of data we want to protect, and the tools used to protect that data.