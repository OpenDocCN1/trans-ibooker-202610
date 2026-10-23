# Getting Started with Postman

There are many desktop applications and browser-based tools for the testing and automation of interaction with HTTP APIs. We are going to use Postman as it has a free version and is one of the more commonly used ones. It’s also great for collaboration. You may be working with a colleague such as an integration consultant or back-end web developer. Being able to quickly build API calls and export them for use by your colleague can be a great productivity boost.

The Postman app can be found here www.postman.com/downloads/. It is available for Mac, Windows, and Linux. There is also a web version of Postman. We shall demonstrate using the desktop app. We will teach just enough Postman to perform the necessary API calls but recommend you check out the Postman Learning Center at https://learning.postman.com/docs/getting-started/introduction/. Figure 1-7 shows the initial view of Postman.

![](bbox=[0.107, 0.386, 0.93, 0.62])

Figure 1-7. Initial View of Postman or by Clicking New

We are going to start by creating a New Environment. This means we won’t have to remember the SuccessFactors API URI. In the future, it will also make it simple to switch between demo or customer environments.

Click “Environment” and then change the name from New Environment to something meaningful like “Mark’s Demo Environment.” Below that, add a variable name of Host and an initial value of the ODATA API URL for your datacenter, such as https://apisalesdemo8.successfactors.com/odata/v2. Add a variable for username, making it user@companyid.