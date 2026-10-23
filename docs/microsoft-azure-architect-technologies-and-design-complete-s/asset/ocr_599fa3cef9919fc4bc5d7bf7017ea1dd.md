# EXERCISE 3.13 (continued)

# FIGURE 3.61 Application Gateway SSL termination template

![](bbox=[0.225, 0.185, 0.764, 0.577])

7. Add a rule by selecting the Rules menu option. Select + Basic to provide a name (for example, CSHARPGUITAR-RULE-HTTP443). Select the listener created in step 3. Select the backend pool from Exercise 3.10, then select the HTTP setting created in step 6, and finally click the OK button.

8. Open a browser and access the Application Gateway virtual appliance’s public IP address prefixed with HTTPS://. You should get an error because the certificate wasn’t created by a trusted certificate authority; it is only a test certificate. Proceed to the site. The output for the request should resemble Figure 3.62.