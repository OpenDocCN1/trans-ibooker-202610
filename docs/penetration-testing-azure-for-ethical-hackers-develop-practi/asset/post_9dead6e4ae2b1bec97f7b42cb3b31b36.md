
For example, if an account locks out after five failed attempts, we will only want to guess three passwords before we wait for the lockout timer to reset. When in doubt, make sure that you work with the people responsible for your penetration test scope to determine an appropriate password guessing strategy:

![](img/4fa338e84e0a590967a142dd798ba4e2_120_0.png)

*Figure 3.19 – Brute-force attack*

A **password spray attack** uses the same password against many usernames (Figure 3.20). One consideration here is to distribute the password guess attempt across many IPs to avoid detection by some security tools (including Microsoft's AD Password Protection and Identity Protection. Links to these can be found in the *Further reading* section, at the end of this chapter).

A tool such as FireProx can be used to rotate source IP addresses by launching the attack using the AWS API Gateway service. Keep in mind that FireProx likely violates the AWS acceptable use policy, which could cause issues for your AWS account:

| FireProx Information | |
|---|---|
| **Creator** | Mike Felch (Twitter: @ustayready) |
| **Open Source or Commercial** | Open source project |
| **GitHub Repository** | https://github.com/ustayready/fireprox |
| **Language** | Python |

![](img/4fa338e84e0a590967a142dd798ba4e2_121_0.png)

*Figure 3.20 – Password spray attack*

Now that we have approaches to use for guessing passwords, let's learn about the tools that we can use to try guessing passwords against our own test environment.

## Introducing MSOLSpray

MSOLSpray is a PowerShell password spraying tool for Microsoft online accounts (Azure/Office365). The advantage of this tool is that, apart from detecting valid passwords, it also gives useful recon information relating to the account's multi-factor authentication (MFA) status, if an account does not exist, if an account is locked or disabled, or if the password has expired.

| MSOLSpray Information | |
|---|---|
| **Creator** | Beau Bullock (Twitter: @dafthack) |
| **Open Source or Commercial** | Open source project |
| **GitHub Repository** | https://github.com/dafthack/MSOLSpray |
| **Language** | PowerShell |

While there are other variants that have been written (MSOLSpray Python, TREVORspray, and so on), we will be using the original MSOLSpray tool, which was written in PowerShell, in the next hands-on exercise.

## Hands-on exercise – guessing Azure Active Directory credentials using MSOLSpray

In this hands-on exercise, we will be using the MSOLSpray tool to perform a password spray attack. Here are the tasks that we will be completing:

- **Task 1**: Create test users in your Azure Active Directory tenant from Azure Cloud Shell.
- **Task 2**: Enable MFA for a test user using the Azure portal.
- **Task 3**: Download and import MSOLSpray on your pentest VM.
- **Task 4**: Create a user list on your pentest VM.
- **Task 5**: Run a password spray attack using MSOLSpray from your pentest VM.

Let's begin:

1. Open a web browser and browse to the Azure portal at https://portal.azure.com. Sign in with your azureadmin credentials.

2. In the Azure portal, click on the Cloud Shell icon in the top-right corner:

    ![](img/4fa338e84e0a590967a142dd798ba4e2_122_0.png)

    *Figure 3.21 – Opening Azure Cloud Shell*

3. If prompted, select PowerShell:

    ![](img/4fa338e84e0a590967a142dd798ba4e2_122_1.png)

    *Figure 3.22 – Selecting "PowerShell"*

    Then, click Create storage:

    ![](img/4fa338e84e0a590967a142dd798ba4e2_123_0.png)

    *Figure 3.23 – Creating a cloud drive for Cloud Shell*

4. In the PowerShell session, within the Cloud Shell pane, run the following command to download a script to provision test users in Azure AD:

    ```powershell
    Invoke-WebRequest -Uri http://bit.ly/az-pentest-create-test-users -OutFile create-test-users.ps1
    ```

    Here's what it looks like:

    ![](img/4fa338e84e0a590967a142dd798ba4e2_123_1.png)

    *Figure 3.24 – Downloading the PowerShell script from our GitHub repository*

5. Verify that the script was successfully downloaded using the following command. You should see a script called create-test-users.ps1:

    ```powershell
    Get-ChildItem
    ```

    Here's what the output looks like:

    ![](img/4fa338e84e0a590967a142dd798ba4e2_123_2.png)

    *Figure 3.25 – Verifying that the PowerShell script has been downloaded*

6. Execute the downloaded script using the following command. When prompted to enter a password, enter myPassword123 and press Enter. The script will provision five new users in your Azure AD tenant with the password that you entered:

    ```powershell
    ./create-test-users.ps1
    ```

    Here's what this looks like:

    ![](img/4fa338e84e0a590967a142dd798ba4e2_124_0.png)

    *Figure 3.26 – Entering a password for the test users*

7. In the script's output, at the end, make a note of the user principal names and the password. You will need them for the password spray attack:

    ![](img/4fa338e84e0a590967a142dd798ba4e2_124_1.png)

    *Figure 3.27 – Making a note of the user principal names and password*

8. Close the Cloud Shell window:

    ![](img/4fa338e84e0a590967a142dd798ba4e2_124_2.png)

    *Figure 3.28 – Closing the Cloud Shell*

9. In the Azure portal, click the Hub menu icon in the top-left corner and select Azure Active Directory:

    *Figure 3.29 – Clicking on "Azure Active Directory"*

10. In the Azure AD console, in the Manage section, select Users and click on Multi-Factor Authentication:

    *Figure 3.30 – Clicking on "Multi-Factor Authentication"*

11. This opens a new Multi-factor Authentication settings window. Select the users adaeze and juan and click Enable to enable MFA for them:

    ![](img/4fa338e84e0a590967a142dd798ba4e2_126_0.png)

    *Figure 3.31 – Enabling MFA for "adaeze" and "juan"*

12. When prompted, click on enable multi-factor auth, and then click on Close:

    ![](img/4fa338e84e0a590967a142dd798ba4e2_126_1.png)

    *Figure 3.32 – Enabling MFA*

13. In your pentest VM, open the PowerShell console, switch directories to your user's directory, and clone the MSOLSpray GitHub repository using the following commands:

    ```powershell
    cd C:\Users\$env:USERNAME\
    git clone https://github.com/dafthack/MSOLSpray.git
    ```

    Here's what this looks like:

    ![](img/4fa338e84e0a590967a142dd798ba4e2_127_0.png)

    *Figure 3.33 – Cloning the MSOLSpray GitHub repository*

14. Import the MSOLSpray PowerShell modules using the following commands:

    ```powershell
    cd .\MSOLSpray\
    Import-Module .\MSOLSpray.ps1
    ```

15. Create a username list using the following command:

    ```powershell
    notepad userlist.txt
    ```

    When prompted to create a new file, click Yes:

    ![](img/4fa338e84e0a590967a142dd798ba4e2_127_1.png)

    *Figure 3.34 – Creating a new user list*

16. Paste the username output that you made a note of earlier into Notepad. Save and close the Notepad document:

    ![](img/4fa338e84e0a590967a142dd798ba4e2_128_0.png)

    *Figure 3.35 – Pasting the usernames created by the script earlier*

17. Run MSOLSpray against the user accounts using the following commands. Specify the password output that you made a note of earlier. You should see a success message for all the accounts, but you should also see the output for the accounts that you enabled MFA for:

    ```powershell
    Invoke-MSOLSpray -UserList ./userlist.txt -Password myPassword123
    ```

    Here's what the output looks like:

    ![](img/4fa338e84e0a590967a142dd798ba4e2_128_1.png)

    *Figure 3.36 – Performing a password spray attack against the user list*

18. Open the Azure AD portal at https://aad.portal.azure.com in an InPrivate or Incognito window. Sign in with Sandra's credentials:

    Username: sandra@<domain_name_suffix>

    Password: myPassword123

19. You should now be able to access the Azure AD portal as Sandra. Also, you should be able to review the information of other users in the Azure AD tenant as Sandra! (This is the default behavior.)

    ![](img/4fa338e84e0a590967a142dd798ba4e2_129_0.png)

    *Figure 3.37 – Accessing the Azure AD portal with Sandra's compromised account*

20. Go back to Azure Cloud Shell, logged in as the azureadmin user. Download the cleanup script to remove the test users that were created earlier and validate the download using the following commands:

    ```powershell
    Invoke-WebRequest -Uri http://bit.ly/az-pentest-cleanup-test-users -OutFile cleanup-test-users.ps1
    Get-ChildItem
    ```

    Here's what the output looks like:

    ![](img/4fa338e84e0a590967a142dd798ba4e2_129_1.png)

    *Figure 3.38 – Downloading the cleanup script*
