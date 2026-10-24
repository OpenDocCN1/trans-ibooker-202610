## 附录 B • 缩略语

**TPM**
可信平台模块

**VMD**
视频移动侦测

**TTL**
生存时间

**VMPS**
虚拟局域网管理策略服务器

**UDP**
用户数据报协议

**VNA**
厂商中立归档

**UETA**
统一电子交易法

**VoIP**
IP语音

**UL**
保险商实验室

**VPLS**
虚拟专用局域网服务

**UPnP**
通用即插即用

**VQP**
虚拟局域网查询协议

**USB**
通用串行总线

**VTP**
虚拟局域网中继协议

**UTM**
统一威胁管理

**WAN**
广域网

**UTP**
非屏蔽双绞线

**WAP**
无线接入点

**VACL**
虚拟访问控制列表

**WBAN**
无线体域网

**VCR**
录像机

**WEP**
有线等效加密

**VLAN**
虚拟局域网

**Wi-Fi**
无线保真

**WLAN**
无线局域网

**WPA**
Wi-Fi保护访问

**WSDL**
Web服务描述语言

**XD**
执行禁用

**XN**
永不执行

**XSS**
跨站脚本

**SMTP**
简单邮件传输协议

**SYN/ACK**
同步/确认

**SNMP**
简单网络管理协议

**TCP**
传输控制协议

**SQLi**
SQL注入

**TCP/IP**
传输控制协议/互联网协议

**SRK**
存储根密钥

**TGS**
票据授予服务器

**SRP**
安全远程密码

**TKIP**
临时密钥完整性协议

**SSH**
安全外壳

**TLD**
顶级域

**SSL**
安全套接层

**TLS**
传输层安全

**SSO**
单点登录

**TO**
输电所有者

**STA**
生成树算法

**TOCTOU**
检查时间到使用时间

**STP**
屏蔽双绞线

**TOP**
输电运营商

**STP**
生成树协议

**TOU**
使用时间

**SYN**
同步

**TPID**
标签协议标识符

## NIST 初步
## 网络安全
## 框架

美国的国家和经济安全依赖于关键基础设施的可靠运行。为了加强这些基础设施的韧性，奥巴马总统于2013年2月12日发布了第13636号行政命令（EO）“改善关键基础设施网络安全”。该行政命令呼吁制定一套自愿性的网络安全框架，为负责关键基础设施服务的组织提供一种“优先、灵活、可重复、基于绩效且成本效益高”的方法来管理网络安全风险。

行政命令中将关键基础设施定义为“对美利坚合众国至关重要的物理或虚拟系统和资产，这些系统和资产的丧失或损毁将对安全、国家经济安全、国家公共卫生或安全，或上述任何组合产生削弱性影响。”由于外部威胁带来的压力日益增大，负责关键基础设施的组织需要一种一致且迭代的方法来识别、评估和管理网络安全风险。

关键基础设施社区包括公共和私人的所有者与运营者，以及在保护国家基础设施中发挥作用的其他支持实体。每个部门都执行关键功能，这些功能由信息技术（IT）、工业控制系统（ICS），以及在许多情况下由IT和ICS共同支持。为了管理网络安全风险，需要清楚地了解特定于IT和ICS的安全挑战与考虑事项。由于每个组织的风险及其对IT和ICS的使用都是独特的，因此框架的实施也会有所不同。

## 保护（PROTECT）

| 类别 | 方法 | 子类别 |
|---|---|---|
| 访问控制（PR.AC） | 对资产及相关设施的访问仅限于经授权的用户、进程或设备，以及经授权的活动和事务。 | PR.AC.1：对授权的设备和用户管理身份和凭据。<br><br>PR.AC.2：对资产的物理访问进行管理和保护。<br><br>PR.AC.3：对远程访问进行管理。<br><br>PR.AC.4：对访问权限进行管理，纳入最小权限和职责分离原则。<br><br>PR.AC.5：保护网络完整性，在适当情况下纳入网络隔离。 |
| 意识与培训（PR.AT） | 组织的员工和合作伙伴接受网络安全意识教育，并接受充分培训，以履行其信息安全相关的职责和义务，且符合相关策略、程序和协议。 | PR.AT-1：所有用户均被告知并接受培训。<br><br>PR.AT-2：特权用户了解其角色和职责。<br><br>PR.AT-3：第三方利益相关者（如供应商、客户、合作伙伴）了解其角色和职责。<br><br>PR.AT-4：高级管理人员了解其角色和职责。<br><br>PR.AT-5：物理和信息安全人员了解其角色和职责。 |

## 项目（PROJECT）续

| 类别 | 方法 | 子类别 |
|---|---|---|
| **维护（PR.MA）** | 工业控制和信息系统组件的维护与修理按照策略和程序执行。 | **PR-MA-1：** 使用经批准且受控的工具，及时执行并记录组织资产的维护和修理。<br><br>**PR-MA-2：** 对组织资产的远程维护须经批准、记录，并以防止未授权访问的方式进行。 |
| **保护技术（PR.PT）** | 管理技术安全解决方案，以确保系统和资产的安全性与韧性，并符合相关策略、程序和协议。 | **PR-PT-1：** 根据策略确定、记录、实施并审查审计/日志记录。<br><br>**PR-PT-2：** 根据策略保护可移动媒体并限制其使用。<br><br>**PR-PT-3：** 控制对系统和资产的访问，纳入最小功能原则。<br><br>**PR-PT-4：** 保护通信和控制网络。 |

## 检测（DETECT）

| 类别 | 方法 | 子类别 |
|---|---|---|
| **异常与事件（DE.AE）** | 及时检测异常活动，并理解事件的潜在影响。 | **DE-AE-1：** 建立并管理网络运营基线以及用户和系统的预期数据流。<br><br>**DE-AE-2：** 分析检测到的事件，以了解攻击目标和攻击方法。<br><br>**DE-AE-3：** 从多个来源和传感器聚合并关联事件数据。<br><br>**DE-AE-4：** 确定事件的影响。<br><br>**DE-AE-5：** 建立事件告警阈值。 |





| Category | Methodology | Sub-Category |
| --- | --- | --- |
| Security Continuous Monitoring (DE.CM) | The information system and assets are monitored at discrete intervals to identify cybersecurity events and verify the effectiveness of protective measures. | DE.CM-1: The network is monitored to detect potential cybersecurity events.<br>DE.CM-2: The physical environment is monitored to detect potential cybersecurity events.<br>DE.CM-3: Personnel activity is monitored to detect potential cybersecurity events.<br>DE.CM-4: Malicious code is detected.<br>DE.CM-5: Unauthorized mobile code is detected.<br>DE.CM-6: External service provider activity is monitored to detect potential cybersecurity events.<br>DE.CM-7: Monitoring for unauthorized personnel, connections, devices, and software is performed.<br>DE.CM-8: Vulnerability scans are performed. |
| Detection Processes (DE.DP) | Detection processes and procedures are maintained and tested to ensure timely and adequate awareness of anomalous events. | DE.DP-1: Roles and responsibilities for detection are well defined to ensure accountability.<br>DE.DP-2: Detection activities comply with all applicable requirements.<br>DE.DP-3: Detection processes are tested.<br>DE.DP-4: Event-detection information is communicated to appropriate parties.<br>DE.DP-5: Detection processes are continuously improved. |

## RESPOND

| Category | Methodology | Sub-Category |
| --- | --- | --- |
| Response Planning (RS.RP) | Response processes and procedures are executed and maintained, to ensure timely response to detected cybersecurity events. | RS.RP-1: Response plan is executed during or after an event. |
| Communications (RS.CO) | Response activities are coordinated with internal and external stakeholders, as appropriate, to include external support from law enforcement agencies. | RS.CO-1: Personnel know their roles and order of operations when a response is needed.<br>RS.CO-2: Events are reported consistent with established criteria.<br>RS.CO-3: Information is shared consistent with response plans.<br>RS.CO-4: Coordination with stakeholders occurs consistent with response plans.<br>RS.CO-5: Voluntary information sharing occurs with external stakeholders to achieve broader cybersecurity situational awareness. |
| Analysis (RS.AN) | Analysis is conducted to ensure adequate response and support recovery activities. | RS.AN-1: Notifications from detection systems are investigated.<br>RS.AN-2: The impact of the incident is understood.<br>RS.AN-3: Forensics are performed.<br>RS.AN-4: Incidents are categorized consistent with response plans. |
| Mitigation (RS.MI) | Activities are performed to prevent expansion of an event, mitigate its effects, and eradicate the incident. | RS.MI-1: Incidents are contained.<br>RS.MI-2: Incidents are mitigated.<br>RS.MI-3: Newly identified vulnerabilities are mitigated or documented as accepted risks. |
| Improvements (RS.IM) | Organizational response activities are improved by incorporating lessons learned from current and previous detection/response activities. | RS.IM-1: Response plans incorporate lessons learned.<br>RS.IM-2: Response strategies are updated. |

## RECOVER

| Category | Methodology | Sub-Category |
| --- | --- | --- |
| Recovery Planning (RC.RP) | Recovery processes and procedures are executed and maintained to ensure timely restoration of systems or assets affected by cybersecurity events. | RC.RP-1: Recovery plan is executed during or after an event. |
| Improvements (RC.IM) | Recovery planning and processes are improved by incorporating lessons learned into future activities. | RC.IM-1: Recovery plans incorporate lessons learned.<br>RC.IM-2: Recovery strategies are updated. |
| Communications (RC.CO) | Restoration activities are coordinated with internal and external parties, such as coordinating centers, Internet Service Providers, owners of attacking systems, victims, other CSIRTs, and vendors. | RC.CO-1: Public relations are managed.<br>RC.CO-2: Reputation after an event is repaired.<br>RC.CO-3: Recovery activities are communicated to internal stakeholders and executive and management teams. |
