# Plan

When you are creating your infrastructure using the command `terraform plan`, Terraform creates an execution plan for you that is based on the code you write via the Terraform config (.tf) files. The execution plan includes the steps Terraform will take to build the requested infrastructure.

# Apply

When you are satisfied with the plan Terraform has created, you can use the `terraform apply` command to create the infrastructure, which will eventually reach the desired state (if there are no conflicts or errors).

# State

Terraform requires a way to store the state of your infrastructure so that it can manage it. The states are stored in text-based *state files* that hold the mapping of your cloud resources to your configuration. There are generally two ways Terraform can store the state of your infrastructure: locally on the system where you initialize it, or remotely. Storing state remotely is the recommended method because it keeps your infrastructure state backed up, and more engineers can come together to build infrastructure since now there is a single source of truth which makes it possible to know the infrastructure’s current state. State files should not be checked in a Git repository as it can contain secrets; you can utilize Azure blob storage instead. Terraform also supports locking by default for state files.