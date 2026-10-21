
## Table 1-4 Power E1080 environment requirements and noise emission

| Property | Environment |  |  |
|  | Operating |  | Non-operating |
|  | Recommended | Allowable |  |
| Temperature | 18.0°C – 27.0°C (64.4°F – 80.6°F) | 5.0°C – 40.0°C (41.0°F – 104.0°F) | 5°C - 45°C (41°F - 113°F) |
| Low-end moisture | -9.0°C (15.8°F) dew point | -12.0°C (10.4°F) dew point and 8% relative humidity | N/A |
| High-end moisture | 60% relative humidity and 15°C (59°F) dew point | 85% relative humidity and 24.0°C (75.2°F) dew point | N/A |
| Relative humidity | N/A | N/A | 8% to 85% |
| Maximum dew point | N/A | N/A | 27.0°C (80.6°F) |
| Maximum altitude | N/A | 3,050 m (10,000 ft) | N/A |
| Maximum noise level | 10.0 B LWA,m a (heavy workload on one fully configured 16-socket four-node system, 35°C (95°F) at 500 m (1640 ft)) | N/A | N/A |

a. Declared level LWA,m is the upper-limit A-weighted sound power level measured in bel (B).

> Note: IBM does not recommend operation above 27C, however, one can expect full performance up to 35C for these systems. Above 35C, the system is capable of operating, but possible reductions in performance may occur to preserve the integrity of the system components. Above 40C there may be reliability concerns for components within the system.

A comprehensive list of noise emission values for various different Power E1080 server configurations is provided by the Power E1080 product documentation. For more information about noise emissions, search for “Model 9080-HEX server specifications” at IBM Documentation.

> Note: Government regulations, such as those regulations that are issued by the Occupational Safety and Health Administration (OSHA) or European Community Directives, can govern noise level exposure in the workplace and might apply to you and your server installation. The Power E1080 server is available with an optional acoustical door feature that can help reduce the noise emitted from this system.
> 
> The sound pressure levels in your installation depend on various factors, including the number of racks in the installation; the size, materials, and configuration of the room where you designate the racks to be installed; the noise levels from other equipment; the room ambient temperature, and employees' location in relation to the equipment.
> 
> Also, compliance with such government regulations depends on various other factors, including the duration of employees' exposure and whether employees wear hearing protection. IBM recommends that you consult with qualified experts in this field to determine whether you are in compliance with the applicable regulations.

## 1.5 System features

This section lists and explains the available system features on a Power E1080 server. These features describe the resources that are available on the system by default or by virtue of procurement of configurable feature codes.

An overview of various feature codes and the essential information also is presented that can help users design their system configuration with suitable features that can fulfill the application compute requirement. This information also helps with building a highly available, scalable, reliable, and flexible system around the application.

### 1.5.1 Minimum configuration

A minimum configuration enables a user to order a fully qualified and tested hardware configuration of a Power system with a minimum set of offered technical features. The modular design of a Power E1080 server enables the user to start low with a minimum configuration and scale up vertically as and when needed.

Table 1-5 lists the Power E1080 server configuration with minimal features.

Table 1-5 Minimum configuration

| Feature | FC | Feature code description | Min quantity |
|---|---|---|---|
| Primary operating system Feature Code | ► 2145 ► 2146 ► 2147 | ► Primary OS - IBM i ► Primary OS - AIX ► Primary OS - Linux | 1 |
| System enclosure | EDN1 | 5U System node Indicator drawer | 1 |
| Flexible service processor | EDFP | Flexible service processor | 2 |
| Bezel | EBAB | IBM Rack-mount Drawer Bezel and Hardware | 1 |
| Processor | ► EDP2 ► EDP3 ► EDP4 | ► 40-core (4x10) Typical 3.65 to 3.90 GHz (max) Power10 Processor with 5U system node drawer ► 48-core (4x12) Typical 3.60 to 4.15 GHz (max) Power10 Processor with 5U system node drawer ► 60-core (4x15) Typical 3.55 to 4.00 GHz (max) Power10 Processor with 5U system node drawer | 1 of either FC |
| Processor activation | ► EDPB ► EDPC ► EDPD | ► 1 core Processor Activation for #EDBB/#EDP2 ► 1 core Processor Activation for #EDBC/#EDP3 ► 1 core Processor Activation for #EDBD/#EDP4 | ► 16 ► 1 if in PEP 2.0 |
| Memory | ► EVC1 ► EVC2 ► EVC3 ► EVC4 | ► 128 GB (4x32 GB) DDIMMs, 3200 MHz, 16 GBIT DDR4 Memory ► 256 GB (4x64 GB) DDIMMs, 3200 MHz, 16 GBIT DDR4 Memory ► 512 GB (4x128 GB) DDIMMs, 2933 MHz, 16 GBIT DDR4 ► 1 TB (4x256 GB) DDIMMs, 2933 MHz, 16 GBIT DDR4 | 8 features of either FC |

| Feature | FC | Feature code description | Min quantity |
|---|---|---|---|
| Memory activation | EMAZ | 1 GB Memory activation for HEX | 50% or 256 GB in PEP 2.0 |
| DASD backplanes | EJBC | 4-NVMe U.2 (7mm) Flash drive bays | 1 |
| Data protection | 0040 | Mirrored System Disk Level, Specify Code | ► 1 if IBM i ► 0 for other OS |
| UPIC cables | EFCH | System Node to System Control Unit Cable Set for Drawer 1 | 1 |

Note: The minimum configuration that is generated by IBM configurator includes more administrative and indicator features.

### 1.5.2 Processor features

Each system node in a Power E1080 server provides four sockets to accommodate Power10 single chip modules (SCMs). Each processor feature code represents four of these sockets, which are offered in 10-core, 12-core, and 15-core density.

Table 1-6 lists the available processor feature codes for a Power E1080 server. The system configuration requires one, two, three, or four quantity of same processor feature, according to the number of system nodes.

Table 1-6 Processor features.

| Feature code | CCIN | Description | OS support |
|---|---|---|---|
| EDP2 | 5C6C | 40-core (4x10) Typical 3.65 to 3.90 GHz (max) Power10 Processor with 5U system node drawer | AIX, IBM i, and Linux |
| EDP3 | 5C6D | 48-core (4x12) Typical 3.60 to 4.15 GHz (max) Power10 Processor with 5U system node drawer | AIX, IBM i, and Linux |
| EDP4 | 5C6E | 60-core (4x15) Typical 3.55 to 4.00 GHz (max) Power10 Processor with 5U system node drawer | AIX, IBM i, and Linux |

The system nodes connect to other system nodes and to the system control unit through cable connect features. Table 1-7 lists the set of cable features that are required for one-, two-, three-, and four-node configurations.

Table 1-7 Cable set features quantity

| System configuration | Qty EFCH | Qty EFCG | Qty EFCF | Qty EFCE |
|---|---|---|---|---|
| 1-node | 1 | 0 | 0 | 0 |
| 2-node | 1 | 0 | 0 | 1 |
| 3-node | 1 | 0 | 1 | 1 |
| 4-node | 1 | 1 | 1 | 1 |

Every feature code that is listed in Table 1-6 on page 16 provides the processor cores, not their activation. The processor core must be activated to be assigned as resource to a logical partition. The activations are offered through multiple permanent and temporary activation features. For more information about these options, see 2.4, “Capacity on-demand” on page 76.

Table 1-8 lists the processor feature codes and the associated permanent activation features. Any of these activation feature codes can permanently activate one core.

Table 1-8 Processor and activation features

| Processor feature | Static activation feature | Static linux only activation feature |
|---|---|---|
| EDP2 | EDPB | ELCL |
| EDP3 | EDPC | ELCQ |
| EDP4 | EDPD | ECLM |

The following types of permanent activations are available:

**Static**
These features permanently activate cores or memory resources in a system. These activations cannot be shared among multiple systems and remain associated with the system for which they were ordered.

**Regular**
A regular static activation can run any supported operating system workload.

**Linux-only**
A Linux-only static activation can run only Linux workloads and are priced less than regular static activations.

**Mobile**
These features permanently activate cores or memory resources. They are priced more than static activation features because they can be shared among multiple eligible systems that are participating in a Power Enterprise Pool. They also can be moved dynamically among the systems without IBM involvement, which brings more value to the customer than static activations.

**Base**
In a Power Enterprise Pool 2.0 (PEP 2.0) environment, systems are ordered with some initial compute capacity. This initial capacity is procured by using base activation features. These activations do not move like mobile activations in PEP 1.0 environment, but can be shared among multiple eligible systems in a PEP 2.0 pool.

**Any OS base**
These base activations are supported on any operating system (AIX, IBM i, or Linux) in a PEP 2.0 environment.

**Linux only base**
These base activations are priced less than any OS base activation, but support only Linux workloads in a PEP 2.0 environment.

A minimum of 16 processor cores must always be activated with the static activation features, regardless of the Power E1080 configuration. Also, if the server is associated to a PEP 2.0, a minimum of one base activation is required.

For more information about other temporary activation offerings that are available for the Power E1080 server, see 2.4, “Capacity on-demand” on page 76.
