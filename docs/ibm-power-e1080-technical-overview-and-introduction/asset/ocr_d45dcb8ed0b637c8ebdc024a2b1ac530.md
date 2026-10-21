Different types of memory activation features that are available for the Power E1080 server are known to the PowerVM hypervisor as total quantity for each type. The PowerVM hypervisor determines the physical DDIMM memory to be activated and assigned to the LPARs.

Similar to processor core activation features, different types of permanent memory activation features are offered on the Power E1080 server. For more information about the available types of activations, 1.5.2, “Processor features” on page 16.

Orders for memory activation features must consider the following rules:

+   - The system must have a minimum of 50% activated physical memory. It can be activated by using static or static and mobile memory activation features.
+   - The system must have a minimum of 25% of physical memory activated by using static memory activation features.
+   - When a Power E1080 is part of a PEP 2.0 environment, the server must have a minimum of 256 GB of base memory activations.

Consider the following examples:

+   - For a system with 4 TB of physical memory, at least 2 TB (50% of 4 TB) must be activated.
+   - When a Power E1080 is part of a PEP 1.0 environment, a server with 4 TB of physical memory and 3.5 TB of activated memory requires a minimum of 896 GB (25% of 3.5 TB) of physical memory activated by using static activation features.
+   - When a Power E1080 is part of a PEP 2.0 environment, a server with 4 TB of physical memory requires a minimum of 256 GB of memory activated with base activation features.

Table 1-11 lists the available memory activation feature codes for Power E1080. The Order type column provides indicates whether the feature code is available for an initial order only, or also with a MES upgrade on a existing server only, or both.

Table 1-11 Memory activation features.

| Feature code | Description | Order type |
|---|---|---|
| EDAB | 100 GB DDR4 Mobile Memory Activation for HEX | Both |
| EDAG | 256 GB Base Memory Activation (Pools 2.0) | Both |
| EDAH | 512 GB Base Memory Activation (Pools 2.0) | Both |
| EDAL | 256 GB Base Memory Activation Linux only | Both |
| EDAM | 512 GB Base Memory Activation Linux only | Both |
| EDAP | 1 GB Base Memory activation (Pools 2.0) from Static | Both |
| EDAQ | 100 GB Base Memory activation (Pools 2.0) from Static | Both |
| EDAR | 512 GB Base Memory activation (Pools 2.0) from Static | Both |
| EDAS | 500 GB Base Memory activation (Pools 2.0) from Static | Both |
| EDAT | 1 GB Base Memory activation (Pools 2.0) MES only | Both |
| EDAU | 100 GB Base Memory activation (Pools 2.0) MES only | Both |
| EDAV | 100 GB Base Memory Activation (Pools 2.0) from Mobile | Both |
| EDAW | 500 GB Base Memory Activation (Pools 2.0) from Mobile | Both |