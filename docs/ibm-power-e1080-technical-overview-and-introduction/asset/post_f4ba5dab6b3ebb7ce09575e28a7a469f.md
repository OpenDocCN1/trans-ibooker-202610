
Table 2-2 lists a historic account of the SMT capabilities that are supported by each implementation of the IBM Power Architecture® since POWER4.

Table 2-2 SMT levels that are supported by Power processors

| Technology | Cores/system | Supported hardware threading modes | Maximum hardware threads per partition |
|---|---|---|---|
| IBM POWER4 | 32 | ST | 32 |
| IBM POWER5 | 64 | ST, SMT2 | 128 |
| IBM POWER6 | 64 | ST, SMT2 | 128 |
| IBM POWER7 | 256 | ST, SMT2, SMT4 | 1024 |
| IBM POWER8 | 192 | ST, SMT2, SMT4, SMT8 | 1536 |
| IBM POWER9 | 192 | ST, SMT2, SMT4, SMT8 | 1536 |
| IBM Power10 | 240 | ST, SMT2, SMT4, SMT8 | 1920ᵃ |

a. PHYP supports a maximum 240 x SMT8 = 1920. AIX support up to 1920 (240xSMT8) total threads in a single partition, starting with AIX v7.3 + Power10.

## 2.1.4 Matrix-multiply assist AI workload acceleration

The matrix-multiply assist (MMA) facility was introduced by the Power Instruction Set Architecture (ISA) v3.1. The related instructions implement numerical linear algebra operations on small matrices and are meant to accelerate computation-intensive kernels, such as matrix multiplication, convolution, and discrete Fourier transform.

To efficiently accelerate MMA operations, the Power10 processor core implements a dense math engine (DME) microarchitecture that effectively provides an accelerator for cognitive computing, machine learning, and AI inferencing workloads.

The DME encapsulates compute efficient pipelines, a physical register file, and associated data-flow that keeps resulting accumulator data local to the compute units. Each MMA pipeline performs outer-product matrix operations, reading from and writing back a 512-bit accumulator register.

Power10 implements the MMA accumulator architecture without adding an architected state. Each architected 512-bit accumulator register is backed by four 128-bit Vector Scalar eXtension (VSX) registers.

Code that uses the MMA instructions is included in OpenBLAS and Eigen libraries. This library can be built by using the most recent versions of GNU Compiler Collection (GCC) compiler. The latest version of OpenBLAS is available at this web page.

OpenBLAS is used by Python-NumPy library, PyTorch, and other frameworks, which makes it easy to use the performance benefit of the Power10 MMA accelerator for AI workloads.

For more information about the implementation of the Power10 processor’s high throughput math engine, see the white paper A matrix math facility for Power ISA processors.

For more information about fundamental MMA architecture principles with detailed instruction set usage, register file management concepts, and various supporting facilities, see Matrix-Multiply Assist Best Practices Guide, REDP-5612.

## 2.1.5 Power10 compatibility modes

The Power10 core implements the Processor Compatibility Register (PCR) as described in the Power instruction set architecture (ISA) version 3.1, primarily to facilitate live partition mobility (LPM) to and from previous generations of IBM Power hardware.

Depending on the specific settings of the PCR, the Power10 core runs in a compatibility mode that pertains to POWER9 (Power ISA version 3.0) or POWER8 (Power ISA version 2.07) processors. The support for processor compatibility modes also enables older operating systems versions of AIX, IBM i, Linux, or Virtual I/O server environments to run on Power10 processor-based systems.

The Power10 processors of the Power E1080 server support the following compatibility modes:

- POWER8: Enabled by firmware level FW810
- POWER9 Base: Enabled by firmware level FW910
- POWER9: Enabled by firmware level FW940
- Power10: Enabled by firmware level FW1010

## 2.1.6 Processor feature codes

Each system node in a Power E1080 server provides four sockets to accommodate Power10 single chip modules (SCMs). Three different SCM types with core density of 10-, 12-, and 15-cores are offered. The processor configuration of a system node is determined by one of three available processor features. Each feature represents a set of four Power10 SCMs of identical core density. All system nodes in a Power E1080 server must be configured with the same processor feature.

Table 2-3 lists the processor feature codes that are available for Power E1080 servers. Power10 processors automatically optimize their core frequencies based on workload requirements, thermal conditions, and power consumption characteristics. Each processor feature code that is listed is associated with a processor core frequency range within which the SCM cores typically operate. Also, the maximum frequency that is supported by the relevant SCM type is indicated.

Table 2-3 Power E1080 processor features

| Feature code | Description |
|---|---|
| EDP2 | 40-core (4x10) Typical 3.65 to 3.90 GHz (max) Power10 Processor with 5U system node drawer |
| EDP3 | 48-core (4x12) Typical 3.60 to 4.15 GHz (max) Power10 Processor with 5U system node drawer |
| EDP4 | 60-core (4x15) Typical 3.55 to 4.00 GHz (max) Power10 Processor with 5U system node drawer |

## 2.1.7 On-chip L3 cache and intelligent caching

The Power10 processor includes a large on-chip L3 cache of up to 120 MB with a non-uniform cache access (NUCA) architecture that provides mechanisms to distribute and share cache footprints across a set of L3 cache regions. Each processor core can access an associated local 8MB of L3 cache. It also can access the data in the other L3 cache regions on the chip and throughout the system.

Each L3 region serves as a victim cache for its associated L2 cache and also can provide aggregate storage for the on-chip cache footprint.

Intelligent L3 cache management enables the Power10 processor to optimize the access to L3 cache lines and minimize cache latencies. The L3 includes a replacement algorithm with data type and reuse awareness. It also supports an array of prefetch requests from the core, including instruction and data, and works cooperatively with the core, memory controller, and SMP interconnection fabric to manage prefetch traffic, which optimizes system throughput and data latency.

The L3 cache supports the following key features:

- Enhanced bandwidth that supports up to 64 bytes per core processor cycle to each SMT8 core.
- Enhanced data prefetch that is enabled by 96 L3 prefetch request machines that service prefetch requests to memory for each SMT8 core.
- Plus-one prefetching at the memory controller for enhanced effective prefetch depth and rate.
- Power10 software prefetch modes that support fetching blocks of data into the L3 cache.
- Data access with reduced latencies.

## 2.1.8 Open memory interface

The Power10 processor introduces a new and innovative open memory interface (OMI). The OMI is driven by 8 on-chip memory controller units (MCUs) and is implemented in two separate physical building blocks that lie in opposite areas at the outer edge of the Power10 die. Each area supports 64 OMI lanes that are grouped in four ports. One port in turn consists of two links with 8 lanes each, which operate in a latency-optimized manner with unprecedented bandwidth and scale at 32 Gbps speed.

The aggregated maximum theoretical full-duplex bandwidth of the OMI interface culminates at 2 x 512 GBps = 1 TBps per Power10 single chip module (SCM).

The OMI physical interface enables low latency, high-bandwidth, technology-agnostic host memory semantics to the processor and allows attaching established and emerging memory elements. With the Power E1080 server. OMI initially supports one main tier, low-latency, enterprise-grade Double Data Rate 4 (DDR4) differential DIMM (DDIMM) per OMI link. This configuration yields a total memory capacity of 16 DDIMMs per SCM and 64 DDIMMs per Power E1080 server node. The memory bandwidth depends on the DDIMM density configured for a specific Power E1080 server.

The maximum theoretical duplex memory bandwidth is 409 GBps per SCM if 32 GB or 64 GB DDIMMs running at 3200 MHz are used. The maximum memory bandwidth is slightly reduced to 375 GBps per SCM if 128 GB or 256 GB DDIMMs running at 2933 MHz are used.
