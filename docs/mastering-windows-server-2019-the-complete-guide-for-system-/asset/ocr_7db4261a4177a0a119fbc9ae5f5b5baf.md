Even though the new template has been **created**, it has not yet been **published**. So at the moment, the CA server will not offer our new template as an option to the clients, even though security permissions are configured for it to do so. The process to publish a certificate template is very quick—only a couple of mouse clicks—but unless you know about the need to do this, it can be a very frustrating experience because nothing in the interface gives you a hint about this requirement.

# Publishing the template

If your **Certificate Templates Console** is still open (the one where we were managing our templates), close it so you are back at the main certification authority management console. Remember how we noticed that the list of available certificate templates that shows up here is much shorter? This is because only these certificate templates are currently published and available to be issued. In order to add additional templates to the published list, including our new one, we simply right-click on the **Certificate Templates** folder and then navigate to **New | Certificate Template to Issue**:

![](bbox=[0.13, 0.42, 0.87, 0.67])

Figure 6.7: Publishing our new template