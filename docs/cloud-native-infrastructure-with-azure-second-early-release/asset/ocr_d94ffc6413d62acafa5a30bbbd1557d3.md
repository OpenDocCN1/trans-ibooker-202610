new configuration it can be continuously integrated into your deployment pipeline.

# Terraform

Terraform is an open source IaC tool from HashiCorp, written in the Go programming language. The word *terraform* means transforming a planet’s environment to closely resemble earth’s atmosphere such that the human species is able to live on it. This analogy holds true when you look at cloud providers as environments that need to be transformed into a manageable infrastructure to help your application take full advantage of the cloud and reach its true potential.

Terraform helps you build, change, and version infrastructure in the cloud in a safe and efficient manner. It uses a declarative syntax, which means you just have to tell it *what* you want to build rather than *how* you want to build it. A major advantage of Terraform is its support for an easy-to-understand domain-specific language (DSL) called HashiCorp language (HCL), which is relatively easy for humans to interpret. Alternatively, you can use JSON for the same purpose. Moreover, Terraform is cloud agnostic, and hence supports multiple cloud providers, which makes it really easy to learn since you don’t have to learn the language (HCL/JSON) again for a different cloud provider. In this section, we will discuss how Terraform can be leveraged to build infrastructure over Azure using IaC philosophy.

For you to understand how Terraform works, first we have to review some basic terminology:

# Providers

A provider in Terraform is generally (but not always) a cloud provider. The provider’s responsibility is to expose the resources (e.g., cloud resources) and interpret the API. Some of the providers supported by Terraform are Azure, Amazon Web Services (AWS), Google Cloud Platform (GCP), and PagerDuty.