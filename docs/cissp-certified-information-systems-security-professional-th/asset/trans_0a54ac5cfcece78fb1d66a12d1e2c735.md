
正如云服务提供商让企业级基础设施面向更广泛的受众可用一样，SDS 能够为资源有限的组织提供更强大的安全能力。与通用系统和平台的集成以及自动化相结合，减少了时间和安全从业人员资源的需求，并可释放现有人员专注于其他问题。SDS 在某些情况下可能是唯一的选择，例如传统安全工具与微服务等新型架构和技术不兼容，或无法部署软件代理进行监控的情况。微软发布了一份文件，概述了其从传统基于边界的安全和传统工具向 SDS 转型的过程。这一变化部分由向云优先的 IT 基础设施转型驱动，该文件阐述了变化背后的战略；可在此处阅读：`microsoft.com/en-us/itshowcase/designing-a-software-defined-strategy-for-securing-the-microsoft-network`。

# 摘要
软件开发安全是现代信息安全的基石，因为信息系统在很大程度上依赖于软件。不幸的是，它也因其重要性认识较晚而受到一定挑战，相较于软件工程领域而言如此，而且还因过于常见的优先考虑交付功能或解决运营问题而非安全性的做法。
软件开发是一项复杂活动，新增需求并不总是受欢迎。然而，在开发信息系统时忽视安全不会带来积极结果，因此与所有安全问题一样，从业人员必须在与安全开发实践、测试和评估相关的成本与收益之间，以及将安全工具集成到开发者实践和环境中的成本与收益之间取得谨慎平衡。
软件开发领域可用的主动风险缓解措施包括记录安全编码标准和指南、实施成熟度模型以提升组织的安全开发能力，以及实施 DevSecOps 和 SOAR 等实践以提供自动化和效率提升。需要考虑的被动风险缓解措施包括设计和执行适当的日志记录和监控策略，以及旨在检测和纠正漏洞的评估和测试计划。
不期望安全从业人员是软件开发者。然而，鉴于软件在现代信息系统中发挥的关键作用，了解软件的工程和开发过程以及为缓解不安全软件带来的风险所需采取的适当控制措施至关重要。

## 索引
*   **A**
*   滥用案例测试, [438](#c06.xhtml#Page_438)
*   访问控制, [379](#c05.xhtml#Page_379), [395](#c05.xhtml#Page_395), [404](#c05.xhtml#Page_404)–408, [614](#c08.xhtml#Page_614)
*   访问控制列表 (ACL), [337](#c04.xhtml#Page_337), [405](#c05.xhtml#Page_405)
*   访问管理生命周期, [409](#c05.xhtml#Page_409)
*   访问矩阵, [171](#c03.xhtml#Page_171)
*   接入点 (AP), [325](#c04.xhtml#Page_325)
*   可访问性，定义, [6](#c01.xhtml#Page_6)
*   账户访问审查, [409](#c05.xhtml#Page_409)–411
*   账户管理, [444](#c06.xhtml#Page_444)–445
*   账户使用审查, [411](#c05.xhtml#Page_411)
*   问责制, [388](#c05.xhtml#Page_388), [396](#c05.xhtml#Page_396)
*   认可，定义, [445](#c06.xhtml#Page_445)
*   收购，定义, [11](#c01.xhtml#Page_11)
*   Active Directory (AD) 服务器, [111](#c02.xhtml#Page_111)
*   临时模式, [328](#c04.xhtml#Page_328)–329
*   地址解析协议 (ARP), [301](#c04.xhtml#Page_301), [354](#c04.xhtml#Page_354)
*   地址空间布局随机化 (ASLR), [181](#c03.xhtml#Page_181)
*   Adleman, Leonard, [240](#c03.xhtml#Page_240)
*   administrative，术语用法, [49](#c01.xhtml#Page_49)
*   管理控制, [16](#c01.xhtml#Page_16), [132](#c02.xhtml#Page_132), [136](#c02.xhtml#Page_136), [443](#c06.xhtml#Page_443)–444
*   行政调查, [49](#c01.xhtml#Page_49)–50
*   行政法，美国, [49](#c01.xhtml#Page_49)
*   高级加密标准 (AES), [194](#c03.xhtml#Page_194), [233](#c03.xhtml#Page_233), [235](#c03.xhtml#Page_235), [239](#c03.xhtml#Page_239)
*   高级持续性威胁 (APT), [467](#c07.xhtml#Page_467)
*   非洲地区知识产权组织 (ARIPO), [38](#c01.xhtml#Page_38)
*   敏捷, [553](#c08.xhtml#Page_553)–557, [581](#c08.xhtml#Page_581)
*   AirCrack, [331](#c04.xhtml#Page_331)
*   AirSnort, [331](#c04.xhtml#Page_331)
*   Akamal, [334](#c04.xhtml#Page_334)
*   算法
    *   加密算法, [225](#c03.xhtml#Page_225)
    *   数字签名算法, [241](#c03.xhtml#Page_241)
    *   椭圆曲线数字签名算法 (ECDSA), [241](#c03.xhtml#Page_241)
    *   国际数据加密算法 (IDEA), [365](#c04.xhtml#Page_365)
    *   Luhn 算法, [255](#c03.xhtml#Page_255)
    *   消息摘要 5 (MD5), [255](#c03.xhtml#Page_255)
    *   NIST 作为研究和推荐合适的加密算法的责任方, [255](#c03.xhtml#Page_255)
    *   安全哈希算法-3 (SHA-3), [255](#c03.xhtml#Page_255)
    *   安全哈希算法 (SHA-1), [196](#c03.xhtml#Page_196), [228](#c03.xhtml#Page_228), [259](#c03.xhtml#Page_259)
    *   密码学中的选择, [226](#c03.xhtml#Page_226)
    *   生成树算法 (STA), [342](#c04.xhtml#Page_342)
    *   流密码加密算法, [231](#c03.xhtml#Page_231)
    *   弱点, [194](#c03.xhtml#Page_194)
*   允许列表, [515](#c07.xhtml#Page_515)
*   Amazon
    *   CloudFront, [334](#c04.xhtml#Page_334)
    *   弹性计算云 (Amazon EC2), [341](#c04.xhtml#Page_341)
    *   硬化和基线配置, [493](#c07.xhtml#Page_493)
*   亚马逊网络服务 (AWS), [212](#c03.xhtml#Page_212), [223](#c03.xhtml#Page_223), [341](#c04.xhtml#Page_341), [494](#c07.xhtml#Page_494)
*   美国注册会计师协会 (AICPA), [25](#c01.xhtml#Page_25)–26
*   美国采暖、制冷与空调工程师学会 (ASHRAE), [267](#c03.xhtml#Page_267), [275](#c03.xhtml#Page_275), [544](#c07.xhtml#Page_544)
*   放大器, [341](#c04.xhtml#Page_341)
*   Anderson, James, [149](#c03.xhtml#Page_149)
*   年度损失期望值 (ALE), [75](#c01.xhtml#Page_75)
*   年度发生率 (ARO), [72](#c01.xhtml#Page_72)
*   年度化损失期望值 (ALE), [72](#c01.xhtml#Page_72)
*   ANSI/BICSI 002-2014，数据中心设计和实施最佳实践, [267](#c03.xhtml#Page_267)
*   Ansible, [112](#c02.xhtml#Page_112)
*   天线, [327](#c04.xhtml#Page_327)–328
*   反恶意软件 (A/M), [518](#c07.xhtml#Page_518)
*   任何即服务 (XaaS), [204](#c03.xhtml#Page_204)
*   API 端点, [613](#c08.xhtml#Page_613)
*   Apple
    *   Find My, [478](#c07.xhtml#Page_478)
    *   硬化和基线配置, [493](#c07.xhtml#Page_493)
*   应用层（OSI 参考模型）, [286](#c04.xhtml#Page_286), [287](#c04.xhtml#Page_287)
*   应用层（TCP/IP 模型）, [301](#c04.xhtml#Page_301)–302
*   应用程序编程接口 (APIs), [213](#c03.xhtml#Page_213), [439](#c06.xhtml#Page_439), [613](#c08.xhtml#Page_613)–618
*   应用安全 (AppSec), [582](#c08.xhtml#Page_582), [588](#c08.xhtml#Page_588)–590
*   应用虚拟化, [356](#c04.xhtml#Page_356)
*   应用层加密, [193](#c03.xhtml#Page_193), [298](#c04.xhtml#Page_298)
*   应用层防火墙, [338](#c04.xhtml#Page_338)
*   应用，在访问控制系统中的, [386](#c05.xhtml#Page_386)–387
*   架构原则, [150](#c03.xhtml#Page_150)–154
*   ARP 欺骗, [289](#c04.xhtml#Page_289)
