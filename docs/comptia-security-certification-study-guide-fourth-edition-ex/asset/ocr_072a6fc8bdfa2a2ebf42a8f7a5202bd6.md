# Biometrics Enrollment

The first time someone uses a biometric device, they go through what is known as the *enrollment process*. With the enrollment process, the biometric data is taken from a biometric reader and then converted to a digital representation. The biometric data is then run through a mathematical operation, and the results of that operation are stored in a database for authentication to the system.

# Efficacy Rates and Biometric Error Types

When evaluating biometric systems, it is important to understand the efficacy rates and the error rates. The *efficacy rate* is how effective the system is at doing its job at giving satisfactory results.

Although biometrics is the most secure method of authentication, it is possible for the biometric system to make mistakes due to the level of detail in the identifying information. The following are the two common types of errors with biometrics:

+   - **Type I** A type I error is known as the *false rejection rate (FRR)* and occurs when the biometric system fails to authenticate someone who is authorized to access the system.
+   - **Type II** A type II error is the opposite of a type I error in that it allows someone to access the system who is not authorized to access the system. This type of error is known as the *false acceptance rate (FAR)*.

Biometric devices are sometimes rated by the percentage of errors that occur using a value known as the *crossover error rate (CER)*. The CER is a number representing when the number of type I errors equals the number of type II errors. For example, if 5 out of 100 authentication attempts are type I errors and 5 out of 100 authentication attempts are type II errors, then the CER is 5. The lower the CER value, the more accurate the biometric system is.

> **Watch** For the exam, be sure to remember the difference between false rejection rate (FRR), false acceptance rate (FAR), and crossover error rate (CER).

# Certificate-Based Authentication

Certificate-based authentication requires the user or client computer to authenticate to the network by presenting a Public Key Infrastructure (PKI) client-side certificate to the authentication system. You will learn about encryption and PKI in Chapters 12 and 13. The authentication system verifies the certificate by checking the following:

+   - Is the certificate from a trusted certificate authority (CA)?
+   - Has the certificate validation period expired?
+   - Has the certificate been revoked?