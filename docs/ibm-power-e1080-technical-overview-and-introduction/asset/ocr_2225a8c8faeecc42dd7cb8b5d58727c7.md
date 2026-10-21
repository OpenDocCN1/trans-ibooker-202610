IBM Power Systems Private Cloud with Shared Utility Capacity solution is supported only on specific POWER9 and Power10 processor-based systems. Power E1080 servers can co-exist with Power E980 systems in the same pool.

A single Power Enterprise Pool 2.0 supports up to 2000 VMs; up to 1000 VMs are supported per HMC. At the time of this writing, up to 200 VMs are supported per Power E1080 server, which is planned to be increased to 750 VMs per Power E1080.

Besides processor and memory metering, Power Enterprise Pools 2.0 enables metering AIX and IBM i software entitlements and SLES and RHEL subscriptions in the pool.

For more information and requirements, see IBM Power Systems Private Cloud with Shared Utility Capacity, SG24-8478.

# 2.4.3 Static, Mobile, and Base activations

Static processor and Memory activations are restricted to a single system. They are enabled by entering a code into the HMC that manages the system. The extra cores or memory are immediately available for use by workloads on the system.

Static activations are available in two versions:

+   ► Static activations: This standard processor or memory activation can run VIOS, AIX, IBM i, and Linux workloads
+   ► Static activations for Linux: This processor or memory activation can run only VIOS and Linux workloads

Mobile activations are ordered against a specific server, but can be moved to any server within the Power Enterprise Pool 1.0 and support any type of application. Mobile activations can be purchased in the initial order or with an MES upgrade.

Base Processor and Memory activations are associated with a single system and initially act as Static activations. When a system is added to a Power Enterprise Pools 2.0 configuration, all of its Base activations are added to the pool's base capacity.

Base activations for processor and memory resources are available in the following types:

+   ► Base activations: This processor or memory activation can run VIOS, AIX, IBM i and Linux workloads. Initially, Base activations act like Static activations until the system is added to a Power Enterprise Pool 2.0. Static and Mobile activations can be converted to Base activations with an MES order.
+   ► Base Linux activations: This Base Processor activation can run only VIOS and Linux workloads.

Table 2-7 lists the Static, Mobile, and Base processor activation features that are available for initial order on the Power E1080 server.

Table 2-7 Static, Mobile, and Base processor activation features

| Processor feature | EDP2 (3.65 - 3.90 GHz 40-core) | EDP3 (3.60 - 4.15 GHz 48-core) | EDP4 (3.55 - 4.00 GHz 60-core) |
|---|---|---|---|
| Static activation | EDPB | EDPC | EDPD |
| Static activation for Linux | ELCL | ELCQ | ELCM |
| Base activation | EPDC | EPDD | EPDS |
| Base activation for Linux | EPDU | EPDW | EPDX |