Perform Exercise 2.5 to enable multifactor authentication.

# Enable Multifactor Authentication

It turns out the conditional access policy created in the previous exercise was too aggressive, and it locked out our partner even though we wanted the account to be authenticated. In Exercise 2.5, you will modify the conditional access policy that blocked all Guest users who have a suspicious sign-in with a calculated value of Low. Instead of blocking the authentication from proceeding, as long as the partner has a valid mobile device that can receive a verification code, we will allow access.

# EXERCISE 2.5

# Enabling Multifactor Authentication

+   1. Log in to the Azure Portal at portal.azure.com.

+   2. Click the Azure Active Directory link, select Conditional Access, click + Select, and select the policy created in the previous exercise, as shown in Figure 2.29. In this case, Block All Guests (LOW)(PORTAL) is selected.

FIGURE 2.29 Enabling multifactor authentication

![](bbox=[0.145, 0.5, 0.865, 0.755])

+   3. In the Access Controls section, select the Grant > Grant Access radio button. Select the Require Multi-factor Authentication check box. Click the Select button and click Save. See Figure 2.30.