Deployment Manager allows us to automate the process of creating and configuring a variety of GCP resources. We can write all the resources our application needs in a declarative format using YAML files, which ensure repeatable deployments with consistent results. YAML is a data sequencing language that humans can easily read. Overall, Deployment Manager can be referred to as Infrastructure as Code (IaC). You may have some experience with tools such as Ansible or Terraform and will recognize that you can nest elements through indentation, rather than any braces or brackets, which are used in some other languages.

> **Exam Tip**
> Deployment Manager is a very large topic, and deep-dive courses are available so that you can gain further knowledge. For the exam, we will explain the fundamental knowledge that's needed to be successful.

Some components are fundamental to Deployment Manager. Configuration files contain all the resources we want to deploy, as well as their properties.

A configuration file starts with the resource string and is followed by a list of entries, each of which has the following fields:

+   - **Name:** The name of the resource we are deploying; for example, `vm001`.
+   - **Type:** This specifies the base type of this resource. There are many available resource types, and they relate to the type of resource we wish to deploy. For example, the resource type for a new VM instance would be `compute.v1.instance`. Note that `v1` stands for production-ready.
+   - **Properties:** This specifies the properties that are required to create the resource. For example, if we wish to deploy a new VM instance from the GUI, we need to populate mandatory fields such as OS type, disk type, and image and network interfaces. These would be populated in the `properties` section of a configuration file.

> **Important Note**
> A full list of resource types can be found here: https://cloud.google.com/deployment-manager/docs/configuration/supported-resource-types.