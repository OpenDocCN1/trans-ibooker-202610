
Thank you for purchasing *Penetration Testing Azure for Ethical Hackers*!

Hack responsibly and good luck!

## Who this book is for

This book is for new and experienced information security practitioners who want to learn how to simulate real-world Azure attacks using tactics, techniques, and procedures that adversaries use in cloud breaches. Any technology professional working with the Azure platform (including Azure administrators, developers, and DevOps engineers) interested in learning how attackers exploit vulnerabilities in Azure-hosted infrastructure, applications, and services will find this book useful.

## What this book covers

*Chapter 1, Azure Platform and Architecture Overview,* covers the basics of how the Azure platform works.

*Chapter 2, Building Your Own Environment,* explains how to create a test environment that can be used in order to follow the hands-on exercises in the book.

*Chapter 3, Finding Azure Services and Vulnerabilities,* explains how to utilize anonymous attacks to find Azure-hosted services and attack them to gain initial access to an environment.

*Chapter 4, Exploiting Reader Permissions,* covers attacks available to users with one of the least-permissioned roles (Reader) in Azure.

*Chapter 5, Exploiting Contributor Permissions on IaaS Services,* explains the available infrastructure attacks that can be executed with the Contributor role.

*Chapter 6, Exploiting Contributor Permissions on PaaS Services,* explains how to attack platform-hosted services with the Contributor role to gain access to credentials, identities, and privilege escalation opportunities.

*Chapter 7, Exploiting Owner and Privileged Azure AD Role Permissions,* covers how to use privileged roles in subscriptions and Azure AD to move laterally and escalate tenant privileges.

*Chapter 8, Persisting in Azure Environments,* explains the goals of persistence and the techniques used by attackers to hide in an Azure environment.

## To get the most out of this book

This book relies on multiple hands-on exercises to guide the reader through the material. Readers of this book will greatly benefit by setting up a test Azure subscription and a supporting virtual machine, outlined in *Chapter 2, Building Your Own Environment*. The authors strongly encourage the reader to utilize the new account credits that are offered by Microsoft to help offset the operating costs associated with running cloud resources.

## Download the color images

We also provide a PDF file that has color images of the screenshots/diagrams used in this book. You can download it here: `http://www.packtpub.com/sites/default/files/downloads/9781839212932_ColorImages.pdf`.

## Download the example code files

You can download the example code files for this book from GitHub at `https://github.com/PacktPublishing/Penetration-Testing-Azure-for-Ethical-Hackers`.

In case there's an update to the code, it will be updated on the existing GitHub repository. We also have other code bundles from our rich catalog of books and videos available at `https://github.com/PacktPublishing/`. Check them out!

## Conventions used

There are a number of text conventions used throughout this book.

`Code in text`: Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles. Here is an example: "A user named `karl` in the Azure AD tenant with a domain name of `azurepentesting.com` will have a UPN of `karl@azurepentesting.com`."

A block of code is set as follows:

```
```json
{
  "assignableScopes": [
    "/"
  ],
```
```

**If you are interested in becoming an author:** If there is a topic that you have expertise in and you are interested in either writing or contributing to a book, please visit `authors.packtpub.com`.

## Share Your Thoughts

Once you've read *Penetration Testing Azure for Ethical Hackers*, we'd love to hear your thoughts! Please click here to go straight to the Amazon review page for this book and share your feedback.

Your review is important to us and the tech community and will help us make sure we're delivering excellent quality content.

## Section 1:
Understanding the
Azure Platform and
Architecture

This section will cover the basics of the Azure ecosystem and explain the attacks that are available from an anonymous, internet-facing standpoint.

This part of the book comprises the following chapters:

- Chapter 1, Azure Platform and Architecture Overview
- Chapter 2, Building Your Own Environment
- Chapter 3, Finding Azure Services and Vulnerabilities

## 1
Azure Platform
and Architecture
Overview

The Azure cloud is Microsoft's public cloud computing platform. The platform consists of multiple services that customers can use to develop, host, and enhance their applications and services. Like many other cloud platforms (**Amazon Web Services (AWS)**, **Google Cloud Platform (GCP)**, and so on), it is constantly growing and evolving by frequently adding new services and features to the ecosystem. Given the availability of all of these cloud services, and the flexibility of Microsoft 365 licensing, many organizations are moving their operations up into the Azure cloud.

In our first chapter, we will focus on providing an overview of the Azure platform, its architecture, the core services, and how those services are managed.

In this chapter, we'll cover the following topics:

- The basics of Microsoft's Azure infrastructure
- An overview of Azure services
- Understanding the Azure **role-based access control (RBAC)** structure
- Accessing the Azure cloud

By the end of the chapter, we will have a good understanding of how organizations use Azure and how to approach an Azure environment as a penetration tester.

## Technical requirements

You won’t need any additional software for most of this first chapter, as we will be focusing on attaining a high-level understanding of the Azure infrastructure. Having Azure portal access to an existing subscription is certainly handy for following along, but not needed for understanding the concepts.

At the end of the chapter, we will review the multiple methods for accessing the Azure management interfaces. So, if you do have access to an existing Azure environment, the following tools will be helpful to have available:

- The Azure **command-line interface (CLI)**
- The Az PowerShell module
- The **Azure Active Directory (Azure AD)** PowerShell module

If you don’t have access to an existing Azure environment, don’t worry. We will go through the steps to creating your own testing environment in *Chapter 2, Building Your Own Environment.*

## The basics of Microsoft's Azure infrastructure

At the time of writing (late 2020–early 2021), the Azure platform consists of over 200 services and seems to be expanding all the time. It may feel that there is a lot of ground to cover here, but during a penetration test, you will typically only need to focus on a subset of the available services that you have in scope.

In general, it is important to understand how the environment is structured at the Azure platform level (subscriptions, RBAC, resources), and how the available services can be abused to gain additional privileges in the environment.

The lessons in this section will be fundamental to your understanding of how Azure functions as a platform, so pay close attention. For those with a solid Azure background, feel free to skim this chapter to refresh on the core principles.

In this section, we will gain an understanding of the Azure cloud platform and its regions, how Azure tenants are typically structured, and how the resources under the tenant are managed.

## Azure clouds and regions

To be able to serve several distinct markets governed by different laws and regulations, Microsoft has built different Azure clouds that cater to different markets. These clouds all run on the same technologies and provide the same services, but they run in different data center environments that are isolated both physically and logically. This is important to keep in mind, as the **application programming interface (API)** endpoints for each platform and their services vary depending on the cloud platform that we are interacting with.

The following table highlights the four Azure cloud platforms and their main endpoints:

| Cloud Platform | Endpoints |
|---|---|
| Azure Commercial | Portal: https://portal.azure.com<br>AD endpoint: https://login.microsoftonline.com |
| Azure United States (US) Government | Portal: https://portal.azure.us<br>AD endpoint: https://login.microsoftonline.us |
| Azure China | Portal: https://portal.azure.cn<br>AD endpoint: https://login.chinacloudapi.cn |
| Azure Germany | Portal: https://portal.microsoftazure.de<br>AD endpoint: https://login.microsoftonline.de |

Since each of these clouds has different endpoints for accessing Azure services and we want to avoid confusion across regions, we will standardize on the Azure Commercial cloud for all the examples in the book. It is important to note that the examples are applicable to the other Azure clouds, but you may need to modify the target endpoints.

## Azure resource management hierarchy

Before we get into the tactics, techniques, and procedures that can be used during penetration tests in Azure environments, we need a working understanding of how resources are structured in the Azure cloud. This knowledge will also give us the ability to follow an attack chain through an environment once we have obtained initial access.
