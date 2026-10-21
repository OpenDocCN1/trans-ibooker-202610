```
-rw------ 1 nsingh nsingh 51 Apr 13 21:13 config
drwrxr-xr-x 1 nsingh nsingh 26 Apr 13 21:13 logs
drwrxr-xr-x 1 nsingh nsingh 10 Apr 13 21:13 telemetry
-rw-r--r-- 1 nsingh nsingh 16 Apr 13 21:13 telemetry.txt
-rw-r--r-- 1 nsingh nsingh 211 Apr 13 21:13 versionCheck.json
```

# Basic usage and infrastructure setup with Terraform

Now that you have successfully set up Terraform to communicate with Azure, you can build some basic infrastructure. As you work your way through these steps, you will be able to connect the dots and use Terraform effectively.

All the Terraform code related to this chapter is available in the book’s GitHub repository (https://bit.ly/3GRlnx7), which you can clone onto your local machine.

Go to the Test directory, and you will find a file named test.tf. Example 2-1 shows the contents of this file. In this example, we are using a local state file to store the current state of the infrastructure.

# Example 2-1. The test.tf file

```
provider "azurerm" {
}
resource "azurerm_resource_group" "rg" {
  name = "testResourceGroup"
  location = "westus"
}
```

The code will create a resource group named testResourceGroup in westus.

To run this file, follow these steps:

+   - Run terraform init to initialize Terraform and download the azurerm provider:

```
$ Test git:(master) terraform init
```

```
Initializing the backend...
```