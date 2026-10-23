
13. In the Enter Connection Info window, enter the following information:

Display name: The account name from Step 8 of this exercise

Account Name: The account name from Step 8 of this exercise

Account Key: The account key from Step 8 of this exercise

Click Next:

![](img/4fa338e84e0a590967a142dd798ba4e2_236_0.png)

Figure 6.17 – Authenticate with the access key

14. In the Summary window, click Connect.

15. Review the files and note that we now have persistent access to the storage account and the content of all its services. This gives a lot of room for an attacker to cause damage. For example, an attacker could encrypt data in the service in a ransomware attack, inject malicious data into NoSQL table stores, or poison applications using storage queues:

### Figure 6.18 – Access to sensitive information in a storage account

While we are not using Lava for this exercise, it is worth noting that Lava has a module called stg_blob_download that will automatically exploit this scenario to download all blob containers in an Azure subscription (Figure 6.19):

```
Lava $> exec_stg_blob_download
[+] getting storage accounts in subscription... [+]
[+] getting the account names [+]
[+] getting the resource group names [+]
[+] getting the storage endpoint names [+]

[+] getting the account keys [+]
[+] getting the storage containers [+]
[+] starting download [+]
[+] files will be saved to /tmp/c2653a362426ec0697ec85a9315828a3 [+]

1/57: "CREDITS.txt"[#####################################]
2/57: "assets/css/font-awesome.min.css"[#####################################]
3/57: "assets/css/main.css"[#####################################]
4/57: "assets/fonts/FontAwesome.otf"[#####################################]
5/57: "assets/fonts/fontawesome-webfont.eot"[#####################################]
```

### Figure 6.19 – Using Lava to download blob files

Beware that some storage containers can contain significant amounts of data, so there is some risk to running this Lava module. Make sure that you know what you are downloading before initiating a download of an entire container. In a pentest engagement, this requires extra care as Personally Identifiable Information (PII) may be present in the accounts.

Now that we have seen how we can maintain a connection to a storage account, let's switch gears within storage accounts to attack Cloud Shell storage files for privilege escalation.

## Attacking Cloud Shell storage files

If you have been following along with the book's examples, you should be very familiar with Azure Cloud Shell (the browser-based command-line interface). But what if we told you that by having you use this shell, we have set you up for failure? We know that you have put a lot of trust in us as authors, and we are truly sorry if we have betrayed that trust. So, let's take a moment to learn from this situation by showing you how to exploit the vulnerable environment that you have created for yourself.

The first time you used Cloud Shell as the azureadmin user, you were prompted to create a file share in the file service of a storage account. If you went with the default option, the platform would automatically create a dedicated storage account with a name prefix of cs to host the file share (Figure 6.20). You could also choose to use an existing storage account to host this share.

![](img/4fa338e84e0a590967a142dd798ba4e2_238_0.png)

Figure 6.20 – Example Cloud Shell storage account

This file share service in the storage account is used to persist data across sessions. Once created, it will be automatically mounted as the clouddrive folder in the $Home directory of the user's subsequent Cloud Shell sessions. You can verify this by running the ls command in Cloud Shell (either Bash or PowerShell) as shown in Figure 6.21:

![](img/4fa338e84e0a590967a142dd798ba4e2_238_1.png)

Figure 6.21 – Mounted file share in a Cloud Shell session

But what about the file share behind this? This share stores an EXT2 filesystem as an .img file.

The name of the share and the image filename usually indicate the Azure AD user that Cloud Shell is attached to. In Figure 6.22, we can see that the cs-azureadmin-azurepentesting-com-1003200107d764bc file share contains the acc_azureadmin.img file (under the .cloudconsole folder). This information can be correlated with the results of Azure AD/RBAC reconnaissance efforts to determine whether the user of the shell is assigned privileged permissions.

![](img/4fa338e84e0a590967a142dd798ba4e2_239_0.png)

Figure 6.22 – Example Cloud Shell file

So, what is the attack scenario here? As we mentioned earlier, this IMG file is mounted to the home directory of the user's Cloud Shell session every time it is launched. Because the Contributor role has permissions (through the storage account keys) to download this IMG file, we could use that access to download and mount the IMG file on a Linux system.

At this point, we can read any sensitive information from the file, but we can also modify it to run commands the next time it is mounted by the target account. If we target the right account, this gives an opportunity to escalate privileges by running commands in the Cloud Shell session of a more privileged user, such as an Azure AD global administrator or a subscription Owner.

> **Important note**
> For a full write up of the original research on this attack, refer to the following NetSPI blog: https://www.netspi.com/blog/technical/cloud-penetration-testing/attacking-azure-cloud-shell/.

For the next exercise, we are assuming that you have been running all the environment setup scripts (as instructed) from Cloud Shell. At a minimum, we will assume that you have already set up a Cloud Shell storage account for the azureadmin user account that you have been using.

## Hands-on exercise – Escalating privileges using the Cloud Shell account

In this exercise, we will exploit a Cloud Shell volume image to run sensitive commands with the permissions of a privileged user when they use Cloud Shell. The process that we will cover will follow a four-step process as highlighted in Figure 6.23. First, we will download the Cloud Shell volume image of a higher privileged user, we will mount the image on the Linux VM that we deployed earlier, we will plant commands that we want to run and re-upload back into the Cloud Shell file share.

![](img/4fa338e84e0a590967a142dd798ba4e2_240_0.png)

Figure 6.23 – Example Cloud Shell file

Here are the tasks that we will complete in this exercise:

- Task 1: Scan Azure file shares at scale using Lava.
- Task 2: Download files in Azure file shares using Lava.
- Task 3: Exploit the Cloud Shell image and upload it back.
- Task 4: Verify the privilege escalation exploit.

Here are the steps to complete the tasks:

- 1. In an RDP session to your Pentest Windows VM, open PowerShell as an administrator.
- 2. In the PowerShell console, type the following commands to SSH into the pentest Linux VM that was created earlier. Replace <linux_FQDN> with the value of Linux VM FQDN from the output of the script earlier. When prompted about the authenticity, type yes and press Enter. When prompted for the password, enter the value of Linux VM Password from the output of the script earlier.
- 3. In the Bash shell of the pentest Linux VM, use the following command to switch to the root user:
- 4. Use the following command to authenticate to Azure as the Contributor user using the Azure CLI. Replace <domain_name> with your Azure AD domain name. Also, replace <contributor_user_password> with the password of the contributoruser account:

```
ssh pentestadmin@<linux_FQDN>
```

Here is a screenshot of the command and its output:

![](img/4fa338e84e0a590967a142dd798ba4e2_241_0.png)

Figure 6.24 – SSH into the Linux VM

```
pentestadmin@ptlinuxvm:~# sudo su -
```

```
az login -u contributoruser@<domain_name> -p <contributor_user_password>
```

Here is a screenshot of the output:

![](img/4fa338e84e0a590967a142dd798ba4e2_242_0.png)

Figure 6.25 – Azure CLI login from the Linux VM

5. Download and run Lava with the following commands:

```
git clone https://github.com/mattrotlevi/lava.git
```

```
cd lava/
```

```
python3 lava.py
```

Here is a screenshot of the output:

![](img/4fa338e84e0a590967a142dd798ba4e2_242_1.png)

Figure 6.26 – Output of the Lava command
