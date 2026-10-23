# Exam Essentials

Know what lift and shift means. This term has to do with moving an application that is running in an on-premise data center and moving it to the cloud without making any changes. Having read this chapter, you will recognize there are some cloud-only issues you may face and some optimal patterns and styles to follow that will improve the chances of being successful in the cloud. They include Retry and creating a reusable static connection object.

Understand authentication protocols. There are numerous methods for protecting your application by checking for credentials: Forms Authentication, certificate authentication, Windows Authentication (Kerberos), and OAuth. These are protocols that protect the application entry point, which then allow the client to execute the code. You need to implement some logic to authenticate and authorize access to the application and to the features running within it.

Understand encryption. This area has to do with the concept of cryptography. The examples provided in this book focused on the encryption of data on a given column in an Azure SQL database table. This is an example of encrypting your data at rest. The data is encrypted when inserted, the data is selected from the database encrypted, and then the client can use an encryption key to decrypt and view the value. This kind of operation can add latency, so implement it only on very sensitive data.

# Key Terms

+   - authentication (AuthN)
+   - authorization (AuthZ)
+   - Azure Front Door
+   - certificate authority (CA)
+   - Command and Query Responsibility Segregation (CQRS)
+   - continuous integration/continuous deployment (CI/CD)
+   - integrated development environments (IDE)
+   - Integrated Windows Authentication (IWA)
+   - Team Foundation Services (TFS)