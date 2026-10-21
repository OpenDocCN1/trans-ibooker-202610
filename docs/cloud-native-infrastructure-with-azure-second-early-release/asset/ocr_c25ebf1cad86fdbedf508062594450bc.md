storage, like Azure blob storage, which provides both durability and safety for your infrastructure configurations.

> WARNING
> 
> Terraform states are the source of truth and should always be stored in a backend such as Consul or Amazon DynamoDB Azure storage because this will safely store your infrastructure state along with version control for easier management or rollback in case of a disaster. States should not be checked in the Git version control system, because Git contains secrets.

In the following subsections, we will create blob storage in our Azure account to store all our infrastructure states. We will also follow the “Don’t repeat yourself” (DRY) principle by ensuring that we make use of Terraform’s module. We will be going over the creation of virtual networks and virtual machines. You can find the modules in the repo https://bit.ly/2ZSVSze.

# Creating Azure blob storage

To create Azure blob storage, you need to use the following module: https://bit.ly/3bM6jCx.

Now, follow the steps outlined in the “Basic Usage and Infrastructure Setup with Terraform” section to deploy the storage account. Once you issue the command terraform plan, you will see a lot of information regarding the plan intertwined with modules. You will also notice that the modules (e.g., Storage_accounts) are abstracted, while the resource file looks like this:

```
module "CloudNativeAzure-strg-backend" {
  source = "../Modules/Services/Storage_accounts"
  resource-grp-name = "CloudNativeAzure-group"
  storage-account-name = "cnabookprod"
  azure-dc = "westus"
  storage-account-tier = "Standard"
  storage-replication-type = "LRS"
  storage-container-name = "cloud-native-devs"
  storage-container-access = "private"
}
```