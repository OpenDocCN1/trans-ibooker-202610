Once executed on the Azure VM in the default backend pool, a request to either of the following URLs would be routed to the Azure VM on that backend pool (for example, CSHARPGUITAR-V2).

```
<application gateway Public IP>/transaction/
<application gateway Public IP>:8080/transaction/
```

As you begin to use this feature more, it becomes clear there are some constraints, in that it is possible to have only one listener per port, which is why port 8080 was used. Each rule must be linked to a unique listener. Figure 3.67 illustrates the hierarchy of dependencies (listener, port, HTTP settings) and the overall request pipeline.

# FIGURE 3.67 Application Gateway configuration hierarchy

![](bbox=[0.32, 0.38, 0.66, 0.62])

# Azure Load Balancer

Having a web application running on multiple dedicated servers or VMs is a common scenario. This design provides redundancy in case one of the servers experiences a transient or fatal system issue. There also could be an application error or resource exhaustion on one server, so having others fill the void in those situations is a good design decision. A load balancer is a network device that is placed in front of all those servers before routing the request to a particular server in the backend pool of VMs. The load balancer exposes a single public IP address and then manages the routing of the request to one of the backend servers.

# OSI Layers and Their Association to Network Appliances

> Azure Firewall runs on OSI layer 3, Application Gateway and Azure Front Door both run on OSI layer 7, and Azure Load Balancer runs on layer 4. The choice of network appliance depends on the application requirements.