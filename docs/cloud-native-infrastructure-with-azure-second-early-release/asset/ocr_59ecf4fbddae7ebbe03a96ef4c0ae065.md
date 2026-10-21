# Azure DevOps and Infrastructure as Code

Azure offers *Azure DevOps*, which provides an end-to-end DevOps toolchain for developing and deploying applications and is primarily a hosted service for deploying CI/CD pipelines. You can use Terraform, Ansible, and Packer together as well, and build multistaged pipelines for your projects. Azure DevOps has a bunch of services, including *Azure Pipeline*, which is primarily used for building, testing, and deploying the CI/CD pipelines. We will not take a deep dive into Azure DevOps since it’s beyond the scope of this book, but you can read more about setting up Azure DevOps in the official documentation.⁹

# Summary

In this chapter, we introduced the steppingstones for building a modern cloud native environment using Azure. You created an Azure account, which will come in handy in upcoming chapters as you gain a deeper understanding of cloud native technologies. You were also introduced to Terraform, Packer, and Ansible: three helpful cloud native orchestrators that will assist you in deploying and managing your cloud infrastructure. Terraform is a cloud-agnostic tool for developing infrastructure as code from the ground up, Packer is used to create machine images for development of immutable artifacts, and Ansible is a configuration management tool.

With this basic understanding of Azure and related technologies under your belt, we can move on to the next chapter, where we will talk about containers, the container registry, and containerizing your application.