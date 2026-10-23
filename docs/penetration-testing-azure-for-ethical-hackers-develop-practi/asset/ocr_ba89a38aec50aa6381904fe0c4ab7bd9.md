![](bbox=[0.12, 0.1, 0.85, 0.42])

Figure 3.20 – Password spray attack

Now that we have approaches to use for guessing passwords, let’s learn about the tools that we can use to try guessing passwords against our own test environment.

# Introducing MSOLSpray

MSOLSpray is a PowerShell password spraying tool for Microsoft online accounts (Azure/Office365). The advantage of this tool is that, apart from detecting valid passwords, it also gives useful recon information relating to the account’s multi-factor authentication (MFA) status, if an account does not exist, if an account is locked or disabled, or if the password has expired.

MSOLSpray Information
Creator: Beau Bullock (Twitter: @dafthack)
Open Source or Commercial: Open source project
GitHub Repository: https://github.com/dafthack/MSOLSpray
Language: PowerShell

While there are other variants that have been written (MSOLSpray Python, TREVORspray, and so on), we will be using the original MSOLSpray tool, which was written in PowerShell, in the next hands-on exercise.