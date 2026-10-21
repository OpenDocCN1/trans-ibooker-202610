

表 2-2 列出了自 POWER4 以来 IBM Power Architecture® 的每个实现所支持的 SMT 功能的历史记录。

表 2-2 Power 处理器支持的 SMT 级别

| Technology | Cores/system | Supported hardware threading modes | Maximum hardware threads per partition |
|---|---|---|---|
| IBM POWER4 | 32 | ST | 32 |
| IBM POWER5 | 64 | ST, SMT2 | 128 |
| IBM POWER6 | 64 | ST, SMT2 | 128 |
| IBM POWER7 | 256 | ST, SMT2, SMT4 | 1024 |
| IBM POWER8 | 192 | ST, SMT2, SMT4, SMT8 | 1536 |
| IBM POWER9 | 192 | ST, SMT2, SMT4, SMT8 | 1536 |
| IBM Power10 | 240 | ST, SMT2, SMT4, SMT8 | 1920ᵃ |

a. PHYP 支持最大 240 x SMT8 = 1920。AIX 从 AIX v7.3 + Power10 开始支持单分区最多 1920 个（240xSMT8）总线程。

## 2.1.4 矩阵乘法辅助 AI 工作负载加速

矩阵乘法辅助（MMA）功能由 Power 指令集架构（ISA）v3.1 引入。相关指令在小矩阵上实现数值线性代数运算，旨在加速计算密集型内核，例如矩阵乘法、卷积和离散傅里叶变换。

为了高效加速 MMA 操作，Power10 处理器核心实现了密集数学引擎（DME）微架构，有效为认知计算、机器学习和 AI 推理工作负载提供加速器。

DME 封装了计算高效的流水线、物理寄存器文件以及将累加器数据保持在计算单元本地的相关数据流。每个 MMA 流水线执行外积矩阵运算，从 512 位累加器寄存器读取并写回。

Power10 在不增加体系结构状态的情况下实现了 MMA 累加器架构。每个体系结构 512 位累加器寄存器由四个 128 位向量标量扩展（VSX）寄存器支持。

使用 MMA 指令的代码包含在 OpenBLAS 和 Eigen 库中。该库可使用最新版本的 GNU Compiler Collection（GCC）编译器构建。OpenBLAS 的最新版本可在该网页上获得。

OpenBLAS 被 Python-NumPy 库、PyTorch 和其他框架使用，这使得 Power10 MMA 加速器在 AI 工作负载中的性能优势易于使用。

有关 Power10 处理器高吞吐量数学引擎实现的更多信息，请参阅白皮书 A matrix math facility for Power ISA processors。

有关基本 MMA 架构原理、详细的指令集使用、寄存器文件管理概念以及各种支持设施的更多信息，请参阅 Matrix-Multiply Assist Best Practices Guide, REDP-5612。

## 2.1.5 Power10 兼容模式

Power10 核心实现了处理器兼容性寄存器（PCR），如 Power 指令集架构（ISA）版本 3.1 中所述，主要用于促进与 IBM Power 硬件前几代之间的实时分区迁移（LPM）。

根据 PCR 的具体设置，Power10 核心以与 POWER9（Power ISA 版本 3.0）或 POWER8（Power ISA 版本 2.07）处理器相关的兼容模式运行。对处理器兼容模式的支持还使 AIX、IBM i、Linux 或 Virtual I/O 服务器环境的旧操作系统版本能够在基于 Power10 处理器的系统上运行。

Power E1080 服务器的 Power10 处理器支持以下兼容模式：

- POWER8：由固件级别 FW810 启用
- POWER9 Base：由固件级别 FW910 启用
- POWER9：由固件级别 FW940 启用
- Power10：由固件级别 FW1010 启用

## 2.1.6 处理器功能代码

Power E1080 服务器中的每个系统节点提供四个插槽以容纳 Power10 单芯片模块（SCM）。提供三种不同核心密度为 10、12 和 15 核的 SCM 类型。系统节点的处理器配置由三个可用的处理器功能之一决定。每个功能代表一组四个核心密度相同的 Power10 SCM。Power E1080 服务器中的所有系统节点必须配置相同的处理器功能。

表 2-3 列出了可用于 Power E1080 服务器的处理器功能代码。Power10 处理器会根据工作负载要求、热条件和功耗特性自动优化其核心频率。每个列出的处理器功能代码都与 SCM 核心通常运行的处理器核心频率范围相关联。此外，还指出了相关 SCM 类型的最大频率。

表 2-3 Power E1080 处理器功能

| Feature code | Description |
|---|---|
| EDP2 | 40-core (4x10) Typical 3.65 to 3.90 GHz (max) Power10 Processor with 5U system node drawer |
| EDP3 | 48-core (4x12) Typical 3.60 to 4.15 GHz (max) Power10 Processor with 5U system node drawer |
| EDP4 | 60-core (4x15) Typical 3.55 to 4.00 GHz (max) Power10 Processor with 5U system node drawer |

## 2.1.7 片上 L3 缓存和智能缓存

Power10 处理器包含一个大型的片上 L3 缓存，最大可达 120 MB，采用非统一缓存访问（NUCA）架构，提供在 L3 缓存区域集合中分配和共享缓存占用空间的机制。每个处理器核心可以访问关联的本地 8MB L3 缓存。它还可以访问芯片上其他 L3 缓存区域以及系统中的数据。

每个 L3 区域充当其关联 L2 缓存的牺牲缓存，并且还可以为片上缓存占用空间提供聚合存储。

智能 L3 缓存管理使 Power10 处理器能够优化对 L3 缓存行的访问并最小化缓存延迟。L3 包含具有数据类型和重用感知的替换算法。它还支持来自核心的一系列预取请求，包括指令和数据，并与核心、内存控制器和 SMP 互连结构协同工作以管理预取流量，从而优化系统吞吐量和数据延迟。

L3 缓存支持以下关键特性：

- 增强的带宽，支持每个 SMT8 核心每个处理器周期高达 64 字节。
- 增强的数据预取，由 96 个 L3 预取请求机启用，为每个 SMT8 核心服务对内存的预取请求。
- 内存控制器的加一预取，以增强有效的预取深度和速率。
- Power10 软件预取模式，支持将数据块提取到 L3 缓存中。
- 具有降低延迟的数据访问。

## 2.1.8 开放内存接口

Power10 处理器引入了新的创新开放内存接口（OMI）。OMI 由 8 个片上内存控制器单元（MCU）驱动，并在 Power10 芯片的相对外边缘的两个独立物理构建块中实现。每个区域支持 64 个 OMI 通道，分成四个端口。每个端口又由两个每个 8 个通道的链路组成，以 32 Gbps 速度以延迟优化的方式运行，具有前所未有的带宽和扩展性。

OMI 接口的聚合最大理论全双工带宽达到 2 x 512 GBps = 1 TBps 每 Power10 单芯片模块（SCM）。

OMI 物理接口使处理器能够以低延迟、高带宽、技术无关的主机内存语义进行操作，并允许连接已建立和新兴的内存元件。Power E1080 服务器。OMI 最初支持每个 OMI 链路一个主层、低延迟、企业级双倍数据速率 4（DDR4）差分 DIMM（DDIMM）。此配置产生每个 SCM 总共 16 个 DDIMM 和每个 Power E1080 服务器节点 64 个 DDIMM。内存带宽取决于为特定 Power E1080 服务器配置的 DDIMM 密度。

如果使用运行在 3200 MHz 的 32 GB 或 64 GB DDIMM，则最大理论双工内存带宽为每个 SCM 409 GBps。如果使用运行在 2933 MHz 的 128 GB 或 256 GB DDIMM，则每个 SCM 的最大内存带宽略微降低至 375 GBps。

