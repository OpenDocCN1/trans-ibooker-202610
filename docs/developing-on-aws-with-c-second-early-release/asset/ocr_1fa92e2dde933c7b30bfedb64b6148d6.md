It is worth mentioning that PowerShell for AWS offers a sophisticated and high-level interface to control and interact with services on AWS, including EC2, S3, Lambda, and more. You can further explore those topics here.

The AWS CloudShell supports three shells: PowerShell, Bash, and Z shell. To use PowerShell, you type `pwsh`. You can explore all AWS CloudShell features using the official documentation. The AWS CloudShell comes with 1 GB of persistent storage in `/home/cloudshell-user`. The action menu, as shown in Figure 1-11, allows you to perform many practical actions, including downloading and uploading files, restarting the terminal, deleting the data in the home directory, and configuring the tabs layout of the terminal.

![](bbox=[0.22, 0.37, 0.78, 0.81])

Figure 1-11. AWS CloudShell Action Menu

Fortunately, the AWS CloudShell PowerShell is straightforward, similar to local PowerShell. First, to use the `AWSPowerShell.NetCore`, import it