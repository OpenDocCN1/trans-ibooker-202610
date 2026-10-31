# Using WFAS instead

The newer platform used for establishing IPsec connection rules is Windows Defender Firewall with Advanced Security. Go ahead and open that up, as you are already familiar with doing. Once inside, navigate to the Connection Security Rules section, which is listed immediately below Inbound Rules and Outbound Rules. Connection Security Rules is where you define IPsec connection rules. If you right-click on Connection Security Rules and choose New Rule..., you will then walk through a wizard that is similar to the one for creating a firewall rule:

![](bbox=[0.13, 0.26, 0.87, 0.46])

Figure 9.24: WFAS can also be used to create IPsec policy rules

Once inside the wizard to create your new rule, you start to see that the options available to you are quite different from the ones shown when creating a new firewall rule. This is the platform from which you will establish IPsec connection security rules that define what the IPsec tunnels look like, and on which machines or IP addresses they need to be active: