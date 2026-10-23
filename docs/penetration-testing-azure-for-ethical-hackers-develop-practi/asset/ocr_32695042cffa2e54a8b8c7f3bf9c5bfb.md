Now that we have seen how we can maintain a connection to a storage account, let's switch gears within storage accounts to attack Cloud Shell storage files for privilege escalation.

# Attacking Cloud Shell storage files

If you have been following along with the book's examples, you should be very familiar with Azure Cloud Shell (the browser-based command-line interface). But what if we told you that by having you use this shell, we have set you up for failure? We know that you have put a lot of trust in us as authors, and we are truly sorry if we have betrayed that trust. So, let's take a moment to learn from this situation by showing you how to exploit the vulnerable environment that you have created for yourself.

The first time you used Cloud Shell as the azureadmin user, you were prompted to create a file share in the file service of a storage account. If you went with the default option, the platform would automatically create a dedicated storage account with a name prefix of cs to host the file share (Figure 6.20). You could also choose to use an existing storage account to host this share.

![](bbox=[0.281, 0.443, 0.718, 0.568])

Figure 6.20 – Example Cloud Shell storage account

This file share service in the storage account is used to persist data across sessions. Once created, it will be automatically mounted as the clouddrive folder in the $Home directory of the user's subsequent Cloud Shell sessions. You can verify this by running the ls command in Cloud Shell (either Bash or PowerShell) as shown in Figure 6.21:

![](bbox=[0.328, 0.708, 0.671, 0.793])

Figure 6.21 – Mounted file share in a Cloud Shell session

But what about the file share behind this? This share stores an EXT2 filesystem as an .img file.