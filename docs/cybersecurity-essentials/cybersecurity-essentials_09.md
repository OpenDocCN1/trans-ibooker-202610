## Infrastructure Security: Review Questions and Hands-On Exercises

*Review the following summary* points before proceeding to the “Review Questions” and “Exam Questions” sections at the end of this chapter to make sure you are comfortable with each concept. After completing the review, answer the review questions to verify your knowledge of the material covered in Part I.

## Summary Points

- *Security* is the science, technique, and art of establishing a system of exclusion and inclusion of individuals, systems, media, content, and objects.
- *Physical security* is the science, technique, and art of establishing a system of exclusion and inclusion for tangible assets. In practice, this involves policies, practices, and steps aimed at combating theft, preventing physical damage, maintaining system integrity and services, and limiting unauthorized disclosure of information.
- *Cybersecurity* involves securing physical access to property, systems, and equipment ports while securing intangible assets including electronic, optical, and informational access to the system’s data and controls.
- *Infrastructure security* refers to physical security initiatives that are applied to providing security for the basic physical and

- A key component that brings all three levels of security together is a well-designed security policy that states how security is implemented at each level.
- A cohesive access control policy at each security level provides authorized people with appropriate levels of access to selected assets, while inhibiting access to assets by people who are not authorized.
- *Authentication* is the process of determining that someone is who they say they are.
- Effective access control involves being able to control the ingress, egress, and regress to an asset based on authorization. In particular, limiting the access of unauthorized personnel to important assets is the most fundamental security objective.
- Multiple factors are involved in authentication:
  - Knowledge—Something that only the designated person should know (something you know)
  - Possession—Something that only the designated person should have (something you have)
  - Inherence—Something that only the designated person is (something you are)
  - Location—Somewhere you are
- Many physical authentication systems are based on single authentication factors that depend on possession.
- Intelligent authentication methods involve *two-factor authentication* (a process that requires two of the factors to grant authorization) based on knowledge and possession.
- *False rejection* or false negative failures are reports that produce an incorrect rejection of the individual, thereby locking them out of a facility or security area to which they should have access.
- *False acceptance* or false positive failures are reports that incorrectly authenticate the individual, which could provide access to equipment or data that this person should not be able to access. Of the two types of authentication failure, this is the most significant in that it could grant access to malicious people.





- *远程监控*是指从远程位置或控制室对设备进行监控或测量。在安全领域，这涉及通过通信系统对安全系统进行外部访问。
- 远程访问监控系统用于在未经授权的访问尝试时通知监督安全人员。
- 由于开/关状态与锁/未锁状态不同，单个传感器无法区分这两种状态。需要安装并监控第二种或不同类型的传感器来执行此区分。
- 远程访问控制是一种设计功能，通过位于访问点以外位置的身份验证系统来验证进入受保护区域的人员身份，从而管理对受保护区域的进入（安全区域或计算机系统）。
- 远程控制访问是一种设计功能，与远程监控系统配合使用，可从远处监控、控制和监督门、闸门和运输工具。
- 功能性入侵检测和报告系统通常包括一个智能控制面板，通过电线或无线电信号连接到设施或组织各处的传感器。
- 每种安全控制器型号都设计为处理特定数量的可编程区域。区域可以是单个保护点，例如运动探测器，也可以将多个点组合成一个区域。
- *传感器*是一类输入设备，可将物理活动转换为可呈现给安全控制器的信号。它们有多种配置可供选择，包括用于门窗的磁性开关、声学探测器、振动探测器、运动探测器和玻璃破碎探测器。传感器保护外围、选定的外部区域以及设施内部的开放空间。
- 物理入侵检测系统通常包括三种基本类型的输出设备：视觉通知、可听式报警器和远程消息。
- 有两种类型的火灾探测传感器可用：热探测器和烟雾探测器。它们通过检测家中温度升高或烟雾来工作。

## 基础设施安全场景 2

识别：___________________________
保护：___________________________
检测：___________________________
响应：___________________________
恢复：___________________________

## 专业反馈

在本节中，您将把自己的观察与一名在职安全专家的观察进行比较——在本例中是 BlackByte 网络安全创始人 Philip Craig——以提高您对网络安全的理解。

## 关于 PHILIP CRAIG

Philip Craig 是 BlackByte Cyber Security, LLC 的创始人，该公司是一家为太平洋西北国家实验室（PNNL）研究和国家安全议程提供支持的咨询公司，以及全国农村电力合作协会和全国农村电信合作协会。

多年来，Phil 在 PNNL 担任高级网络安全研究科学家，在网络安全、监控控制与数据采集（SCADA）技术、计算和通信基础设施领域提供工程和项目管理支持。

这包括在核电、电力和水等多个关键基础设施领域开发复杂的系统和政策解决方案。他为电力和核电行业开发并部署了战略性和战术性网络安全防御解决方案。

## 实践专业人士的见解

实践中的安全专业人员在确定许多部署中最有效的解决方案时具有显著优势。在反复在实地实践该行业后，您也将能够创建某些在未来仍有效的模型。

这里有一个经过时间考验的方法，以三个非常基本的问题开头，可让您进入初始评估阶段。务必向客户询问：

- 1. 我们试图保护什么？
- 2. 我们试图保护它免受谁的侵害？
- 3. 我们为什么需要提供保护？

第一个答案始终是一个物理事物（例如某种材料、组件、产品等），您可以实际看到、品尝、触摸和闻到。它可能是在开发中的敏感设备或仪器，也可能具有很高的货币价值。它也可能是对环境敏感的某种材料，或者如果不使用正确的保护方法，可能对公众有害。

第二个答案集中在一个人（潜在的对手）。

最后一个答案可能源于支持公司经济实力的业务需求，或遵循特定法规的要求。毋庸置疑，您可以将尽可能多的新学到的技术应用于解决方案，但如果没有从这些问题中得出的信息，您就会浪费大量时间——更重要的是，浪费客户的金钱。

让我们回顾一下您的场景。考虑以下结构：

- ► 一栋建筑（包含多个楼层和空间）
- ► 一个办公环境（包含办公室和隔间空间）
- ► 一个隔间（包含计算资源）

正如您之前所学到的那样，存在许多物理和网络安全方面的考虑。它们存在于外部、内部和内部环境中，具有许多影响各方访问的属性。我们需要考虑这些影响，并开始提供通常称为边界或分界线的物理和逻辑分离。

图 5.1 代表了一种可靠的模型，可为处理这些考虑提供一致的方法。出于安全目的，我们始终主要关注威胁，因此这种以威胁为依据的模型将始终适用。

- **分析：**必须执行的那些活动（威胁向量分析、攻击树、后果和易损性分析），以评估潜在攻击者成功达到目标的最佳、最可能、最有效或最可能的方式，以及此类成功的影响描述。
- **定义威胁环境：**定义威胁环境代表了金字塔“上部”和“中部”区域上方主题相关的所有属性的汇集。

## 保护底部区域

威胁感知模型底部区域指出的活动包括：

- **安全策略（检测、威慑、拒绝、延迟、响应、恢复）：**基于定义的威胁环境，制定这些策略以确保安全功能能够威慑、检测、拒绝、延迟、响应和从攻击中恢复。
- **安全控制 网络/物理（管理、运营、技术/警卫、闸门、锁）：**为确保安全策略有效而采用的机制。
- **风险判定 – 政策 – 培训 – 审计与合规：**为记录措施以确定整体安全计划有效性而必需的支持性计划要素。

组织往往太快地应用全面的安全政策，然后建立一个确保政策得到满足的计划。从实践安全专业人员的角度来看，这完全与应有的做法相反。然而，在您第一份工作或任何新工作上，您很可能会进入一个运营安全计划。重要的是您仍然采取这种方法，否则您将难以理解决策已经做出背后的原因。

那么现在您已经获得了一种理解什么、谁和为什么的方法，以及一个能够进行良好流程来评估和评估安全环境模型的手段，下一步是什么？您如何着手完成任务？

## 着手解决问题

首先，您需要确定自己的视角。我们称之为环境地图上的“您在此处”点。有两种不同的视角：由外而内和由内而外。想象一座城堡。在城堡盛行的日子里，它们实际上是建造来阻止人们进入的巨大防御结构。这种视角是“由外而内”的视角。建筑师们忙于建造方法


这种方法从外部视角保护了他们的城堡免遭攻破。尽管许多物理安全方法仍然采用这一视角（而且应该如此），但更全面的做法是采用“由内而外”的视角。这种方法将确保最内部的区域得到考量，并且你能够以分级方法构建安全态势，直至覆盖到最外部的区域。这被称为分级方法。它允许安全专业人员根据需要规定安全控制措施，从而避免过度规定、产生过多成本或消耗不必要的资源或精力。

## 我在保护什么？

记录对象、物资和财产。它可以是一盒钻石，也可以是像桑德斯上校的肯德基炸鸡配方那样的知识产权。

## 我保护资产是为了防范谁？

你来扮演对手！从由内而外的视角出发，从最内部的区域（隔间）开始，寻找任何可能挑战你安全控制措施的痕迹。确保你熟悉并了解物理路径：从小隔间到办公区域，再到建筑物本身。同时寻找物理和网络层面的入口与出口。始终像对手一样思考（即位于我们三角形顶端的人）。

## 我为什么要提供保护？

是因为资产的价值吗？是因为某个生产过程一旦中断就可能导致数百万美元的收入损失吗？是因为某些材料可能对员工或公众的安全构成挑战吗？是否存在监管要求或其他法律或合同约束性要求？

## 执行你的计划

优先确定并选择适当的（必要的）安全控制措施，以检测、威慑、阻止、延迟、响应并恢复你的安全态势。应制定记录完善的安装流程和程序，以确保安全控制措施得到正确安装。按照既定方案实施你的计划。确保所有物理和网络措施均按要求落实到位。按照程序检查实施情况。投入运行后，需要建立一种持续检查的方法，以确保安全控制措施始终有效。这些控制措施通常涵盖从简单的内部网络钓鱼演练、摇晃门窗检查牢固性，到实际开展旨在测试响应和缓解能力的网络/物理联合挑战演练。在执行定期评估和演练的过程中，持续改进你的安全态势；这些活动可能暴露弱点，并提供纠正或增强能力的机会。如果没有这一循环，你将永远无法捍卫你的运营预算，也无法获得管理层的支持。

根据任何公司或组织的“什么、谁以及为什么”，有数百甚至数千种方法可以提供安全和可信的环境。成功的方法往往是那些你能够以充分论据加以论证且记录完善的方法。未来的雇主不会仅仅因为你能力出色而留用你，他们会因为你做事严谨周密而留用你。

## 复习题

以下问题评估你对第一部分内容的掌握情况。

- 1. ______________ 是建立有形资产排除与纳入体系的科学、技术和艺术。
  答案：物理安全。在实践中，这涉及旨在打击盗窃、防止物理损坏、维护系统完整性和服务，以及限制未经授权信息泄露的政策、实践和步骤。
- 2. ______________ 是一种错误认证个人身份的报备，这可能会让该人获得本不应拥有的设备或数据访问权限。
  答案：错误接受或误报（假阳性）失败
- 3. 请说明适用于监控摄像头的勒克斯额定值，并描述这些设备的典型勒克斯额定值范围。
  答案：获得合理的摄像机图像所需的光照量称为勒克斯额定值。勒克斯是衡量照射到物体上的光量的单位。1勒克斯约等于距离1米处的一根蜡烛照射在1平方米面积上的光量。典型摄像机的额定值范围在0.5至1.0勒克斯之间。
- 4. 利用结构和景观等自然设计元素来引导人们进出空间的做法被称为 ____________。
  答案：自然出入控制
- 5. 哪种类型的安全设备用于编程、控制和操作出入控制和管理设备？
  答案：大多数入侵检测与报告系统使用键盘设备来编程、控制和操作各种出入控制和管理设备。
- 6. 哪种类型的摄像机在低光照条件下能够提供最佳分辨率？
  答案：红外摄像机。红外安防摄像机的镜头外圈装有红外LED照明灯（其发出的光线来自我们通常不常看到的电磁波谱区域）。这种照明使摄像机在完全无光的环境下也能拍摄出良好的图像。在微弱光线（称为低光照）下，红外摄像机可以拍摄出看起来如同白昼的画面。
- 7. 在旨在生成最高质量图像的摄像机中，使用的是哪种类型的图像传感器？
  答案：CCD（电荷耦合器件）。最好的监控摄像机采用电荷耦合器件（CCD）技术。它们具有高分辨率、低工作光照要求、较低的温度依赖性和高可靠性。
- 8. 描述安全系统中键盘的主要用途。
  答案：大多数入侵检测与报告系统使用键盘设备来编程、控制和操作各种出入控制和管理设备。
- 9. 描述用于向关键人员或远程监控机构报告报警状态的技术。
  答案：最常见的远程通知系统是入侵检测与报告系统的控制面板利用电话线路，在出现报警状态时自动呼叫远程监控中心或关键人员。有些系统使用独立的电话拨号器或内置拨号器。然而，越来越多的系统具备内置蜂窝通信系统。这类系统提供了额外的可靠性，即使物理电话线路受损也能正常工作。
- 10. ________________ 利用结构、系统和设备来防止未经授权的进入，并在公共与私人空间之间建立明确界限。
  答案：领域强化
- 11. 通过 ________________，状态监控系统可以记录并提示每次特定大门或门被解锁（授予访问权限）的时间以及授予了哪种类型的访问权限。解锁监控还可以识别被授予访问权限的人员。
  答案：解锁状态监控
- 12. 列出入侵检测与报告系统中周边区域输入传感器的典型安装位置。
  答案：控制面板的周边区域输入通常包括每个周边开口处的传感器，如门、窗、车库门和窗，以及通往狭小空间的入口门。额外的周边保护措施可能包括使用声音、振动和运动探测器传感器，以防止通过破损窗户进入。
- 13. 在安全控制器上创建物理安全区域时使用了哪种物理技术？
  答案：将相关传感器按逻辑分组以创建安全区域。具体做法是将所有相关传感器开关（在安全控制器看来，所有传感器都表现为开关）以串联方式连接起来，并连接到控制器面板上的一组特定触点。
- 14. 列出认证系统中常用的四个因素。
  答案：可用于建立身份认证的因素有多种：知识——你知道的事物，拥有——你拥有的物品，固有——你自身的特征，以及位置——你所处的位置。
- 15. 说出与存储视频监控信息（尤其是在大型企业中）相关的两个主要问题。
  答案：需要存储多少视频？需要存储多长时间？这些问题的答案使组织能够确定其存储容量需求。

## 考试题

1. 保护下列哪一项涉及控制谁可以跨越界定该周界的物理或逻辑边界线（如产权线或建筑物/综合体的外墙）移动（步行、驾车、飞行）？
- A. 内部空间
- B. 内周界
- C. 外周界
- D. 主要区域

答案：C




2. 以下哪一项不是基础设施安全管理中涉及的子系统？
A. 访问控制和监控系统
B. 入侵检测和报告系统
C. 视频监控系统
D. 企业网络安全政策
答案：D

3. 以下哪些选项代表物理屏障？（多选。）
A. 上锁的门
B. 前台接待员
C. RFID 徽章读卡器
D. 监控摄像头
答案：A 和 B

4. 哪种类型的监控摄像头可以从世界几乎任何地方查看？
A. 数字相机
B. 数字 IP 相机
C. 模拟相机
D. 混合相机
答案：B

5. 从以下报告类型中，哪些选项会导致对个人的错误拒绝，从而将其锁定在原本应有权访问的设施或安全区域之外？（多选。）
A. 误拒
B. 误接纳
C. 假阴性失败
D. 假阳性失败
答案：A 和 C

6. 哪种传感器检测光束（可见或不可见）并对接收到的光强度的变化做出响应？
A. 微波传感器
B. 压力传感器
C. 运动传感器
D. 光电传感器
答案：D

## Exam Questions

7. 哪种镜头使你能够查看整个房间但图像会产生一定失真？
A. 鱼眼镜头
B. 长焦镜头
C. 定焦镜头
D. 变焦镜头
答案：A

8. 以下哪一项最能描述照度等级在监控摄像头中的含义？
A. 相机镜头尺寸的等级
B. 获得可接受图像所需的光量
C. 相机镜头的分辨率
D. 指定相机的色彩分辨率
答案：B

9. 以下哪种摄像头通过使用超出可见光谱的照明来提供保持一定保密性的能力？
A. CCD 相机
B. 红外安防摄像头
C. 黑白相机
D. 彩色相机
答案：B

10. 以下哪种摄像头最适合用于监控 24/7 运行的自动取款机，该机器必须在白天和低水平夜间照明条件下运行，同时提供高分辨率、详细的视图以监控机器所使用的不同银行功能？
A. 相机 1 – 800 × 600 像素分辨率，1.0 lux 照度等级
B. 相机 1 – 2240 × 1680 像素分辨率，0.5 lux 照度等级
C. 相机 1 – 1024 × 768 像素分辨率，0.75 lux 照度等级
D. 相机 1 – 1536 × 1180 像素分辨率，0.9 lux 照度等级
答案：D
