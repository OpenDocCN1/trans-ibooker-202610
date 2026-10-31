# Building your own auxiliary module

The Metasploit Framework has almost 1,000 auxiliary modules at the time of writing, and the number is always rising, because there will always be new software and vulnerabilities that are still not available in the framework. For that reason, in this recipe, we will learn how to build our own auxiliary module.

# Getting ready

In this recipe, we will write an auxiliary module that will scan for Huawei home routers with **CPE WAN Management Protocol (CWMP)** enabled. CWMP is a protocol used by providers for remote management of customer-premises equipment. It allows auto-configuration, software or firmware image management, software module management, status and performance management, and diagnostics.

# How to do it...

+   1. When we connect to the router using the CWMP default port 7547, we get the following answer:

![](bbox=[0.13, 0.48, 0.87, 0.635])