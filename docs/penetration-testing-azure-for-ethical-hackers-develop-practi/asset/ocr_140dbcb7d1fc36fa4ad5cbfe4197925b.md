# # Creating a new Azure tenant

This chapter will guide you through both the setup of a sample environment and a pentest VM in your own Azure subscription. In order to create a VM to complete the hands-on exercises in this chapter, you need to have an Azure tenant. If you do not have an Azure tenant, follow the steps in this section to set up your own. If you already have an Azure tenant, you may want to create a new subscription to use for following these examples.

> **Important note**
> 
> If you intend to use an existing subscription for the examples in the book, make sure that the subscription is separate from any production workloads, systems, and services. The example services that are created in these setup scripts will create vulnerabilities that may expose your environment to risk. Proceed at your own risk!

In an ideal world, this entire environment would be a one-click deployment, but you will benefit from building up the environment yourself. It can be a lot of fun to attack a pre-built **capture the flag (CTF)** environment, but building your own from the ground up will give you a much deeper understanding of the environment that you're working with.

# ## Hands-on exercise: Creating an Azure tenant

At the core of every Azure environment is the tenant. As we covered in the previous chapter, this is where all the subscriptions and **Active Directory (AD)** users will be stored.

Let's look at the tasks we will need to complete, as follows:

+   - **Task 1:** Authenticate to Microsoft.
+   - **Task 2:** Select the free trial option.
+   - **Task 3:** Enter billing information.
+   - **Task 4:** Access a new Azure tenant.