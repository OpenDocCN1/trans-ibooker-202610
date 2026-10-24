

![](img/68b1a140c4426ceff0475b2d902d98df_184_0.png)

**Figure 6.10** 需求的流转。

![](img/68b1a140c4426ceff0475b2d902d98df_184_1.png)

**Figure 6.11** 产品个体视角与集成视角。

lifecycle these responsibilities remain for procurement and test. The task of the system integrator is to ensure that the original concept of an integrated system is maintained so that when the total system is built up during the test phase it is fit for purpose. This means that all agreements made at the concept phase, which include functional separation, functional interfaces, accuracy and resolution of data, timing of data, redundancy, etc., are maintained at all times.

![](img/68b1a140c4426ceff0475b2d902d98df_186_0.png)

**Figure 6.13** 引导与控制示例。

- Communications management: the management of all internal and external communications.
- Displays and controls management in which the design of the cockpit or flight deck and all its constituent systems is integrated to provide an ideal crew working environment.

## 6.3.5 信息层面的集成

通过记录飞机开发的每个阶段来控制生命周期中的产品。图 6.10 所示流程中的开发每个阶段都必须记录下来，以显示需求的流转、与设计的关联，以及通过测试和建模获得的证据，以证明最终产品是安全、可靠且符合用途的。

以这种方式收集的信息对于向客户和监管机构证明飞机可以安全飞行，不会对操作人员和地面人口构成危险至关重要。通过配置控制来行使对产品的控制。这意味着所有模型、图纸、报告、分析以及机型的零件的发放都被记录在案。任何对机型记录的偏差或修改都以受控的方式引入。该任务通常由总工程师或首席设计师负责。

## 6.3.6 主承包商层面的集成

主承包商对系统集成的理解涵盖了上述所有内容以及更多内容。它关注的是在整个生命周期内管理提供满足客户需求的产品的所有方面。

系统集成商对整个产品及其各部分如何协同工作负责。

飞机的成功关键因素在于其满足其作战角色和环境的程度。这不能通过关注车辆的任何一个属性来实现——它取决于完整的系统包的特性，包括机组人员、车辆及其内部子系统、训练和支持系统，以及在军用飞机的情况下武器和支持的军事基础设施。

军方客户关注的特性包括生存能力、致命性和全生命周期成本。商业客户关注的是可用性、采购价格和运营成本。这些特性很大程度上取决于系统元素以何种详细方式组合在一起。这些任务包括：

- tracking, understanding, and influencing the customer requirement from its earliest conceptual stage
- capturing the requirement in a structured manner and flowing down that requirement through and across all aspects of the product definition, manufacture, and operation
- ensuring that the requirements are correctly interpreted and traceable to the design solution
- ensuring that the design is consistent across all constituent sub-systems and their hardware, software, firmware, and human engineering solutions
- conducting the testing and proving of the design at unit, component, sub-system, and system levels, including models and simulations, to gather evidence that the design is sound, robust, safe, and fit for purpose
- compiling and controlling a complete record of the design, including all assumptions and calculations.

## 6.3.7 源于涌现属性的集成

这里描述了一个涌现属性的例子，以说明飞机的子系统如何通过可能未在初始设计中预见到的相互作用而变得“集成”[Moir and Seabridge 2008]。图 6.14 显示了飞机系统中耗散能量产生的热流。该图的关键如下：

1. Air extracted from the engine fan casing is used to cool engine bleed air.
2. Ram air is used to cool engine oil in a primary oil cooler heat exchanger.
3. Fuel is used to cool engine oil in a secondary oil cooler heat exchanger.
4. The electrical integrated drive generator (IDG) oil is cooled by ram air.
5. Hydraulic return line fluid is cooled by fuel before being returned to the reservoir.
6. Fuel is cooled by an air/fuel heat exchanger.
7. Ram air is used in primary heat exchangers in the air-conditioning packs.
8. Ram air is used in secondary heat exchangers in the air-conditioning packs.

在某些极端情况下，例如以军用隐身飞机为例，这种热能的最终排放可能会损害飞机的热特征。必须寻求替代的抛热机制，以降低被寻热导弹探测的威胁。

![](img/68b1a140c4426ceff0475b2d902d98df_188_0.png)

类似的相互作用可以通过分析闪电击中时机身结构和蒙皮中的电流流动、分析机身在系统组件的连接和接地中的电流流动，以及分析结构载荷来观察。这些效应在金属和复合材料混合的结构中尤其有趣。

另一个例子涉及燃油系统以及需求和指示必须流经的计算机和数据总线路径的数量。图 6.15 显示了一个燃油系统传感器向驾驶舱显示器发送信息以及随后机组操作到达执行器的路径的示例，例如油箱燃油不足的指示和飞行员要求转移燃油。

在分层数据总线系统[联邦系统]中集成数据流可能会在数据延迟或“陈旧数据”方面产生一些有趣的结果。每次数据在数据总线之间交换时，数据都可能陈旧，因为数据只能按周期刷新。这种数据“陈旧性”可以在系统设计中解决，但需要认识到该问题。

图 6.15 顶层描绘中显示了三种典型的交易类型。对于燃油系统示例，可以表征如下：

- **Intra-system data transfers:** these are shown as transfers 1 to 4 on Figure 6.15 where data is shared on a system basis without the pilot being involved. Such data transfers could involve a fuel transfer pump being commanded to automatically top-up certain fuel tanks as fuel is expended – an interchange between the fuel gauging and transfer systems.

**Figure 6.15** An example of data latency in an integrated sub-system.

- System to cockpit: transfers 5 and 6 relate to the transfer of data from the system to the cockpit, perhaps relating to fuel on-board (FOB) and the contents of individual fuel tanks when demanded by the pilot.
- Cockpit to system transfers: pilot selections such as demanded fuel transfers 7 and 8 are input into the system via cockpit interface units whereby discrete pilot fuel mode selections are fed into the system computer.

可以很容易地看出，涉及飞行员和系统交互的复杂操作可能依次涉及上述所有交易类型。作为系统时延影响的一个例子，如果每个时延平均为 10 ms，则从传感器到警告再到需求返回到转移泵的潜在总时延可能高达 180 ms。必须在此基础上加上机组人员对警告做出反应、决定做什么然后选择转移所需的时间。在某些情况下，这种延迟可能构成安全隐患。还有其他类型的系统，其中这种数据延迟将完全不可接受，并可能导致系统不稳定。

## 6.3.8 集成系统的进一步示例

### 6.3.8.1 机身

不出所料，机身是一个相互连接和集成的网络。许多航空航天组织的部门结构倾向于将这些线索视为

![](img/68b1a140c4426ceff0475b2d902d98df_189_0.png)

