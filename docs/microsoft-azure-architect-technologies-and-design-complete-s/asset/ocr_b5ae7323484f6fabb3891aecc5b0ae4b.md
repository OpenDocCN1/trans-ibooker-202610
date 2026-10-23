The most common scenario to deploy an Azure VM is the one used in Exercise 3.3. In that scenario, the VM, by default, was allocated an instance level IP. In this case, you are provided with all the expected ephemeral ports within the default range (see Figure 3.19).

# **FIGURE 3.19**  netsh default dynamic port range output from an Azure VM

![](bbox=[0.236, 0.22, 0.752, 0.378])

Execute the following command on an Azure VM to get the output shown in Figure 3.19:

```
```text
netsh int ipv4 show dynamicport tcp
```
```

It is possible to create an Azure VM that does not have an ILIP and instead have the public IP address mapped to the availability set. (I will discuss these in the next chapter.) This approach is often used to secure the VMs from being individually accessible from the internet, instead exposing them via another network device like a load balancer. In that scenario, your SNAT ports are allocated based on the size of the pool of VMs in your availability set. Table 3.8 shows those default SNAT port allocations.

# **TABLE 3.8**  SNAT Allocation per VM Pool Size

| SNAT Ports per IP Address | Number of VMs in Pool |
|---|---|
| 1024 | 1 to 50 |
| 512 | 51 to 100 |
| 256 | 101 to 200 |
| 128 | 201 to 400 |
| 64 | 401 to 800 |
| 32 | 801 to 1000 |