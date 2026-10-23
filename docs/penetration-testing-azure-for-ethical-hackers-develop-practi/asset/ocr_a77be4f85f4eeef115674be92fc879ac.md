The name of the share and the image filename usually indicate the Azure AD user that Cloud Shell is attached to. In Figure 6.22, we can see that the cs-azureadmin-azurepentesting-com-1003200107d764bc file share contains the acc_azureadmin.img file (under the .cloudconsole folder). This information can be correlated with the results of Azure AD/RBAC reconnaissance efforts to determine whether the user of the shell is assigned privileged permissions.

![](bbox=[0.12, 0.25, 0.82, 0.46])

# Figure 6.22 – Example Cloud Shell file

So, what is the attack scenario here? As we mentioned earlier, this IMG file is mounted to the home directory of the user's Cloud Shell session every time it is launched. Because the Contributor role has permissions (through the storage account keys) to download this IMG file, we could use that access to download and mount the IMG file on a Linux system.

At this point, we can read any sensitive information from the file, but we can also modify it to run commands the next time it is mounted by the target account. If we target the right account, this gives an opportunity to escalate privileges by running commands in the Cloud Shell session of a more privileged user, such as an Azure AD global administrator or a subscription Owner.

> **Important note**
> For a full write up of the original research on this attack, refer to the following NetSPI blog: https://www.netspi.com/blog/technical/cloud-penetration-testing/attacking-azure-cloud-shell/.

For the next exercise, we are assuming that you have been running all the environment setup scripts (as instructed) from Cloud Shell. At a minimum, we will assume that you have already set up a Cloud Shell storage account for the azureadmin user account that you have been using.