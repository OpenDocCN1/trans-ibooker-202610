# 1.1.4 System capacities and features

With any initial orders, the Power E1080 supports up to four system nodes. The maximum memory capacity that is supported in each node is 4 TB.

The maximum number of supported PCIe Gen 3 I/O expansion drawer is four per system node. Each I/O expansion drawer can be populated with two Fanout Modules. Each Fanout Module in turn is connected to a system node through one PCIe x16 to CXP Converter Card.

Memory features #EMC1 128 GB, #EMC2 256 GB, #EMC3 512 GB, and #EMC4 1024 GB are available.

The following characteristics are available:

+   - Maximum of 4 #EDN1 5U system node drawers
+   - Maximum of 16 TB of system memory per node drawer
+   - Maximum of 16 #EMX0 PCIe Gen 3 I/O expansion drawers
+   - Maximum of 32 #EMXH PCIe Gen 3 6-slot Fanout Module for PCIe Gen 3 expansion drawers
+   - Maximum of 32 #EJ24 PCIe x16 to CXP Converter Cards

# 1.2 System nodes

The full operational Power E1080 includes one SCU and one, two, three, or four system nodes. A system node is also referred to as a central electronic complex (CEC), or CEC drawer.

Each system node is 5U rack units high and holds four air-cooled Power10 single-chip modules (SCMs) that are optimized for performance, scalability, and AI workloads. An SCM is constructed of one Power10 processor chip and more logic, pins, and connectors that enable plugging the SCM into the related socket on the system node planar.

The Power E1080 Power10 SCMs are available in 10-core, 12-core, or 15-core capacity. Each core can run in eight-way simultaneous multithreading (SMT) mode, which delivers eight independent hardware threads of parallel execution power.

The 10-core SCMs are ordered in a set of four per system node through processor feature #EDP2. In this way, feature #EDP2 provide 40 cores of processing power to one system node and 160 cores of total system capacity in a 4-node Power E1080 server. The maximum frequency of the 10-core SCM is specified with 3.9 GHz, which makes this SCM suitable as a building block for entry class Power E1080 servers.

The 12-core SCMs are ordered in a set of four per system node through processor feature #EDP3. In this way, feature #EDP3 provides 48 cores capacity per system node and a maximum of 192 cores per fully configured 4-node Power E1080 server. This SCM type offers the highest processor frequency at a maximum of 4.15 GHz, which makes it a perfect choice if highest thread performance is one of the most important sizing goals.