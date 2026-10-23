Exercise 3.4 that this curl code was from CSHARPGUITAR-VM to CSHARPGUITAR-V1. In this scenario, you are testing the connectivity between CSHARPGUITAR-V2 and CSHARPGUITAR-V1. See Figure 3.28.

```
curl -G <Public IP address>:80
curl -G <Private IP address>:80
```

# FIGURE 3.28 Using curl to test Azure VM accessibility

![](bbox=[0.24, 0.252, 0.77, 0.395])

12. As expected, using curl for the public IP address works, but not the private IP address. Remember that CSHARPGUITAR-V2 is in San Antonio, Texas, and CSHARPGUITAR-V1 is in Dublin, Ireland. Let's peer the VNets together so the private IP address will work. Navigate to CSHARPGUITAR-VNET-B, click Peerings, and then click + Add.

13. Provide a name for the peering (for example, CSHARPGUITAR-PEER-B2A). In the Virtual Network drop-down list, select CSHARPGUITAR-VNET-A and then name the peering in the other direction (for example, CSHARPGUITAR-PEER-A2B). Leave the remaining defaults and click the OK button.

14. Once the configuration has completed, access CSHARPGUITAR-V2 again and use curl. See Figure 3.29.

# FIGURE 3.29 Using curl to test Azure VM accessibility

![](bbox=[0.315, 0.69, 0.7, 0.788])

15. Execute the same PowerShell cmdlet as in step 10, and you will notice there is a route added to the table (Figure 3.30).