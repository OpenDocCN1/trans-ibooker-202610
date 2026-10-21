# Chapter 5. Creating a Kubernetes Cluster in Azure

# A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 5th chapter of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at rfernando@oreilly.com.

Now that you understand the basics of how a Kubernetes cluster works under the hood, it’s time to put that knowledge to the test and actually build and use a Kubernetes cluster. Several tools are available for creating and running a Kubernetes cluster in a production environment with high availability. Some of the more common tools are kops, kubeadm, Kubespray, and Rancher. These tools have prewritten playbooks on building a cluster that can be run in a production environment seamlessly with a combination of technologies.

We will not use a prebuilt tool in this chapter. Instead, we will take a more hands-on approach to building a cluster that you can use in a production environment.

# Creating a Kubernetes Cluster from Scratch

In this section, we will build a Kubernetes cluster from scratch using Ansible, Terraform, and Packer. We will show you how to manually build the cluster to give you a better understanding of the cluster’s various components. While this is not the preferred way to build or run a production-grade Kubernetes cluster, it will provide you with good foundational knowledge.

To get started, you’ll need to clone the GitHub (https://bit.ly/3kccjZZ) for this book and open the Chapter5 folder. This chapter assumes you have already set up your Azure account by following the steps in Chapter 2, and that you have experimented with creating the infrastructure using the Terraform, Packer, and Ansible playbooks.

We arranged the code in the Chapter5 Git repository as follows: