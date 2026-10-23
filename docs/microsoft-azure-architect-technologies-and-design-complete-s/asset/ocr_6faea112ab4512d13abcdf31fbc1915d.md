# **FIGURE 7.12** An encrypted data element on a database table

![](bbox=[0.15, 0.138, 0.862, 0.235])

At the moment, the web page is open and accessible to anyone who comes across the page or discovers the endpoint. To prevent this, implement EasyAuth to make sure only authenticated users associated with an Azure Active Directory tenant can view the page. See Exercise 7.2.

# EXERCISE 7.2

# Enabling Easy Auth

+   1. Log in to the Azure Portal at portal.azure.com.
+   
+   2. Navigate to the Azure App Service where you configured Managed Identity and Azure Key Vault in Exercise 7.1. Click the Authentication/Authorization link in the navigation menu, set App Service Authentication to On, and select Azure Active Directory from the Authentication Providers section.
+   
+   3. Select Express from the Management mode selection area, set Grant Common Data Services Permissions to On, click the OK button, and finally click the Save button. The configuration should resemble Figure 7.13.

# **FIGURE 7.13** Configuring EasyAuth in Azure App Service

![](bbox=[0.149, 0.614, 0.862, 0.94])