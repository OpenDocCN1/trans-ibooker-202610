
It is important that IT professionals not only remain current with the new features, products, and technologies, but that they learn about what already exists and go deeper into how the technology actually works, all the way down to the bits. IT professionals need to learn in both forward and backward directions; otherwise you might miss why a feature was created in the first place.

## Recognize Azure Product Names, Features, and Functionalities

The more products and features that pass in front of your eyes and get stored into your subconscious, the easier it will become to answer certain types of questions. For example, assume this question is presented to you in one of the exams:

- Which of the following are considered an Azure Storage product or feature? (Choose all that apply.)
- 
- A. Blob Storage
- B. Data Storage
- C. Queue Storage
- D. File Storage

Having some experience with Azure Storage accounts, you might remember that Azure Storage accounts support multiple service types. The types of storage services are Blob, Files, Tables, and Queue. Based on that knowledge, it is for certain that options A, C, and D are the correct answers. But are you certain that there is no Azure product or feature called Data Storage? During the exam you must recollect if you have ever seen, heard, or read about an Azure Data Storage product or feature. You need to be confident in choosing the correct answer. In fact, there isn’t an Azure product or feature called Data Storage in this context. You can check all the Azure products here to be sure:

docs.microsoft.com/en-us/azure/#pivot=products&panel=all

Continuing on that same thought process of name recognition, once you gain enough experience with the kinds of Azure products that exist, then learning what they do and what they are used for is the next step. Knowing the functional purpose of the product or feature will help to answer questions like the following:

- Which of the following Azure products provide messaging management capabilities? (Choose three.)
- 
- A. Service Bus
- B. Event Hub
- C. ExpressRoute
- D. Queue Storage

Service Bus, Event Hub, and Queue Storage are all products that receive messages from endpoints for later processing. ExpressRoute is not a messaging product; rather, ExpressRoute allows an on-premise network to connect over a private link into the Azure cloud using a connectivity provider. Therefore, the answer is A, B, and D.

In conclusion, knowledge of and experience with a large majority of all existing Azure products and features are needed. Without knowing the products that exist, there is no chance of knowing what they do. Knowing both what products exist and what they do is a necessity to achieve the Azure Solutions Architect Expert certification.

## Strive for a Deep Knowledge of a Few, Some Knowledge of Many, and a Basic Knowledge of All

Up to now, the words *product* (aka service) and *feature* have been used numerous times. There is a subtle difference between them. When the word *product* is used, it refers to an object like Azure Virtual Network, Azure App Service, Azure Web Apps, or Azure SQL. The product is something that is bought; it is the item that is selected after clicking the + Create A Resource link in the portal, as shown in Figure 1.4.

![](img/95043b3df926bcdadadb082b49262037_51_0.png)

The products are also in the Favorites section, also shown in Figure 1.4.

*Features*, on the other hand, are additional, usually optional, capabilities that can be added to the product. For example, it is recommended that the Application Insights *feature* is combined with an Azure App Service web app. Also, there is an Azure Search feature that works well with Azure SQL. Backup is not a product and cannot be used on its own; it is a feature that is bound to a specific product. Not all features are available on all products;

some scenarios wouldn’t make sense such as having the Backup feature for Event Hub. Other features such as SSL/TLS or custom domain features for an Azure Function make good sense.

From an exam perspective, it is important to have a broad knowledge of all Azure products and features. The following list shows how many products fall into different Azure categories:

- Compute (13)
- Networking (13)
- Storage (14)
- Web (7)
- Mobile (6)
- Databases (12)
- Security and identity (19)
- Containers (8)

That is a large number of products/services to comprehend, and the number is continuing to grow. The recommended approach is to pick a few of the products that are of most interest to you (Azure VM, Azure App Service, Azure Functions, Azure Database, Azure Security, and so on). Then, based on personal preference, create and configure those products and study them deeply to become an expert in them. Experience how those products interact with each other and learn all the available features available to them. The knowledge gained using this approach, via creating and configuring Azure products and integrating them with other products and features, leads to mastering a few and knowing others rather well.

I gathered the previous list of categories from here:
docs.microsoft.com/en-us/azure/#pivot=products&panel=all
This list contains the Azure products that relate mostly to the Azure Solutions Architect Expert exams. You will learn how to configure and integrate some of the Azure products and features in later chapters.

Do not think that it is required to know all products and features down to the bits to pass the Azure exams. However, deep knowledge in the security, compute, network, and databases categories is expected. This is obvious in the outlines of the exams on the Microsoft Certification website.

```
docs.microsoft.com/en-us/learn/certifications/exams/az-303
```

```
docs.microsoft.com/en-us/learn/certifications/exams/az-304
```

Therefore, learn a few products deeply and learn something about the other many products. You will be expected to at least notice if a product is presented as an Azure product but is not. You can then exclude it from the possible correct answers.

Next, read more about the products and features in the next section. These are the ones that are highly recommended to know in detail.

## An Introduction to “Must-Know” Azure Features

Recognizing a feature name and its functions can help you pass the Azure Solutions Architect Expert exams. This section discusses some of the more well-known and utilized Azure products in a little more detail. Later in the book, you will get the opportunity to create and utilize these products as well as numerous other Azure products in a more complete scenario.

### Azure Active Directory and Security

Security is one of the most important aspects of an IT solution. If you cannot manage the access and manage who has what rights on the system, then it is unlikely the solution will have much value. Azure provides many products and features to help create, enforce, maintain, and monitor your features, from a security perspective.

Whether you realize it or not, the first significant product created when you begin consuming Azure products and features is an Azure Active Directory (AAD), sometimes referred to as a tenant. The first time you set up an Azure subscription, a requested option is the initial domain name. This name must be unique on Azure and is prefixed with *.onmicrosoft.com, where * is the unique name provided by the creator. The tenant must always be sent to AAD when requesting authentication.

> Azure Active Directory also can be referred to as an identity as a service. AAD is used for authenticating sign-ins and identity protection.

Let’s take a step back and touch quickly on how AAD came to be. The first authentication technique was a simple user ID and password data store. However, this broke down because each application typically had their own data store and an account was required for each application. As the number of systems being used increased, so did the maintenance and complexity of gaining access to an application. Therefore, the Active Directory concept was created to store all corporate user credentials, user profiles, and permissions in a central global location. (We are discussing enterprise authentication at this point, not private user accounts.) Active Directory worked just fine; all applications implemented with Integrated Windows Authentication (IWA) could log in to any system on an intranet, without having to sign in multiple times as users moved from one application to the next.

Since the size of a typical corporation is relatively small when compared to all possible users accessing the internet, Active Directory worked well internally. There have been attempts to create a global authentication provider for the internet; however, those attempts have failed.

What Azure Active Directory does is to provide those same authentication and authorization capabilities but on the Azure platform. The Azure Active Directory tenant can be used to link and authenticate software as a service (SaaS) capabilities like Bing, Office 365, or Dynamics CRM. Additionally, AAD can be synchronized with an on-premise Active
