# ## Application Security Best Practices

Let’s first look at some common best practices to help create a more secure application infrastructure. Although some of these topics have been touched on in earlier chapters, it is important to review them:

+   - **Key management** Ensure that the application is creating encryption keys and storing them in a secure manner.
+   - **Credential management** Ensure that any security credentials are used in the application in a secure manner and that they are also stored in an encrypted format within a database.
+   - **Authentication** Ensure that your application is authenticating all users of the application and controlling what a user has access to based on that authentication.
+   - **Geotagging** Verify whether the application is using any geotagging features that store geographical identifying information with media such as photos or video. This could be a privacy concern, as physical location could be determined from this geotagged data.
+   - **Encryption** Ensure the application is encrypting any sensitive information, including network communication and sensitive data in storage.
+   - **Application whitelisting** Depending on the purpose of the application, verify that it has a whitelisting feature, which is an allow list that can be configured in the application. For example, an e-mail program can have a contact whitelist, which is a listing of contacts that are allowed to be e-mailed within the software.
+   - **Transitive trust/authentication** This refers to a feature that allows your application to trust security principles that are trusted by another environment due to the fact that your application trusts the other environment.

# ## BYOD Security Concerns

A big security concern these days is the fact that employees are bringing their own personal devices such as cell phones, tablets, and laptops into the office and using them to perform company tasks. The following represent some concerns with allowing an employee to use a personal device and should be considered as you design your BYOD policy:

+   - **Data ownership** You want to ensure that if you allow employees to use their own personal device for work, you make it clear that the information is owned by the company and not the employee. If the employee leaves the company, they will have to erase all company data from the device.
+   - **Support ownership** It is important to make it clear who is responsible for support of the device if something goes wrong with it. Most companies will ensure the employee knows they are responsible for the support of their own device.
+   - **Patch management** Make it clear who is responsible for keeping the device up to date with patches. Again, this may be looked at as the job of the employee, and it should be made clear to the employee that they must keep the patch level up to date on the device.