We can also see that each VM instance has been deployed to the correct zone:

![](bbox=[0.23, 0.13, 0.77, 0.47])

Figure 16.20 – Instances

This is how we can configure a template for VM instance types only. We can also extend our template to other types of services, such as networking. As a final example, let's look at how to create a template and configuration file to create a networking and firewall ruleset. In this example, we need to have three separate templates: one for our network, one for our subnetwork, and one for our firewall rule.

Let's look at our network first. This is pretty simple. Note that we are now using a type of compute.v1.network and that we have single properties. We set this to false to avoid automatically creating a corresponding subnetwork. The name is a variable that's passed from our configuration file. Let's save this file as network.jinja:

```
resources:
- name: {{ env["name"] }}
  type: compute.v1.network
  properties:
    autoCreateSubnetworks: false
```