Great, we were able to bypass UAC, and we got a new Meterpreter session. As you can see, bypassing UAC is easy, which is why you should not rely on UAC as a security mechanism.

# Dumping the contents of the SAM database

Security Accounts Manager (SAM) is a database in the Windows operating system that contains usernames and passwords; the passwords are stored in a hashed format in a registry hive either as an LM hash or as an NTLM hash. This file can be found in %SystemRoot%/system32/config/SAM and is mounted on HKLM/SAM. In this recipe, you will learn about some of the most common ways to dump local user accounts from the SAM database.

# Getting ready

We will start in a Meterperter session in the Metasploitable 3 target machine, with system privileges running.

# How to do it...

+   1. First, we will start with the classic Meterpreter hashdump command:

![](bbox=[0.22, 0.59, 0.78, 0.83])