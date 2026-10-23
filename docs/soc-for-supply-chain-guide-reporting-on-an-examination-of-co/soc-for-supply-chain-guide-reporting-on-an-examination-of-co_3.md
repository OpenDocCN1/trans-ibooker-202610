## 附录 A

## 给实体管理层的信息

> *本附录不具有权威性，仅供参考。*
> 
> 本文件旨在帮助实体管理层理解其在 SOC for Supply Chain 检查中的责任。它也旨在为实体管理层在履行这些职责时提供有用的指导。

## 引言与背景

许多实体，如下列所述，生产、制造或分销产品：

- *生产商。* 生产商包括通过从地球开采金属、矿物和骨料（如石油和天然气开采、采矿、疏浚和采石）来提取原材料的实体；通过种植某些植物和饲养家养动物（牲畜）来生产食品、饲料、纤维和其他产品；以及开发用于现场安装的软件。
- *制造商。* 制造商包括使用人力和机器、工具、化学和生物过程、制造或配制，将原材料或组件转化为其他组件或成品以供使用或销售的实体。这些组件或成品可能出售给其他制造商，用于生产其他产品，如飞机、计算机或计算机零件、家用电器、家具、运动器材或汽车。在其他情况下，成品可能出售给批发商，批发商再将其出售给零售商，零售商再将其出售给最终用户和消费者。制造商还包括为其他实体外包制造的合同制造商。
- *商业软件开发商。* 商业软件开发商是开发并销售待售商业软件的实体。商业软件开发商不同于软件开发服务提供商，后者根据服务合同为满足特定实体的需求而创建、修改和实施软件。提供软件开发服务的系统最好由 SOC 2® 检查来处理。
- *分销公司。* 分销公司包括提供或管理另一实体全部或大部分物流的实体，包括以下一项或多项：进货运输、清关、仓储、库存管理、订单履行（包括拣选和重新包装物品）、分销或出货运输。此类公司包括第三方物流（3PL 或 TPL）公司。

由于技术的快速发展，产品的生产、制造或分销通常涉及高度的相互依赖，以及



实体与其（a）为制造过程提供原材料或零部件的组织（供应商）¹ 和（b）实体的客户和业务合作伙伴之间的连通性。这些关系通常被视为 *供应链* 的一部分。

尽管这些关系可能会增加收入、扩大市场机会并降低实体的成本，但它们也会给实体开展业务的供应商、客户和业务合作伙伴带来额外风险。因此，这些供应商、客户和业务合作伙伴有责任将其作为供应链风险管理计划的一部分来识别、评估和应对额外风险。这些风险可能会威胁到实体实现以下目标的能力：

- 提供符合主要产品性能规范的产品。
- 满足交付和质量承诺及要求。
- 满足生产、制造或分销承诺及要求。

因此，供应商、客户和业务合作伙伴期望实体管理层建立运营和合规目标。这些目标在本指南中被称为 *系统目标*，并且由于风险的变化以及法律法规的变化而可能随时间变化。

为了识别、评估和应对实体与其用于生产、制造或分销产品的系统之间交互所产生的风险，供应商、客户和业务合作伙伴通常需要了解系统内控制²的设计、运行和有效性信息。为了支持其风险评估，他们可能会要求实体提供鉴证报告。此类报告是鉴证业务的结果，在该业务中，执业人员检查并发表意见，说明（a）描述实体生产、制造或分销产品的系统（*系统描述* 或 *描述*）的描述是否如描述标准所述呈现了设计和实施的系统，以及（b）描述中所述的控制措施，这些控制措施对于合理保证实体实现了其主要系统目标是必要的，是否基于适用的受托服务标准而有效。此项检查被称为 SOC for Supply Chain 检查。

本文件旨在

- 协助实体管理层准备其制造、生产或分销系统的描述，该描述作为 SOC for Supply Chain 检查的基础。
- 使实体管理层熟悉其在聘请服务审计师执行 SOC for Supply Chain 业务时的责任。

¹ 在本文件中使用的 *供应商* 是指个人或企业（及其员工），其向生产商、制造商或分销公司（实体）提供产品（如原材料、零部件或其他商品）或服务。例如，服务提供商就是一种特定的供应商，为实体提供服务。

² 在本文件中，*控制* 是指实体内部控制体系的一部分的政策和程序。实体内部控制体系的目标是合理保证系统目标得以实现。

## SOC for Supply Chain 报告的预期使用者

SOC for Supply Chain 报告旨在为预期使用者提供有关生产、制造或分销产品的系统以及该系统内控制有效性的信息（即与安全、可用性、处理完整性、保密性或隐私等一个或多个适用的受托服务类别相关的控制），这些控制对于基于适用的受托服务标准合理保证实体的主要系统目标得以实现是必要的。该报告旨在为预期使用者提供他们可能用来识别、评估和管理与实体关系所产生的风险的信息。

SOC for Supply Chain 报告旨在供那些对实体、其生产、制造和分销的产品以及生产、制造或分销这些产品的系统具有足够知识和理解的人使用。预期使用者的预期知识通常包括以下内容：

- 实体生产、制造或分销的商品的性质
- 内部控制及其局限性
- 适用的受托服务标准
- 可能威胁实体主要系统目标实现的风险以及控制如何应对这些风险

如果没有此类知识，使用者可能会误解报告的内容、实体管理层的断言以及执业人员的意见，所有这些都包含在 SOC for Supply Chain 报告中。因此，执业人员的报告要求限制在具备该知识的预期使用者。此外，实体管理层和执业人员通常会就报告的预期使用者达成一致。

以下预期使用者被推定为具备所需知识：

- a. 业务客户，包括直接客户或供应链下游的类似业务实体，其执行以下操作：
  i. 将系统的产品用作其生产和制造系统的零部件（例如，生产机械）
  ii. 将系统的产品用作其产品的投入（例如，用于汽车的计算机）
  iii. 将系统的产品作为其服务交付的一部分使用（例如，医院使用的输液袋）
  iv. 转售产品
  v. 依赖物理分销系统用于作为其他产品投入的产品

业务客户需要有关实体系统的信息，包括该系统内控制的性质和有效性，以便了解实体的控制措施，并确定这些控制措施加上其自身的控制措施是否足以减轻其业务风险。

- b. 业务合作伙伴，其
  i. 依赖实体销售业务合作伙伴的商品，或
  ii. 将业务合作伙伴的知识产权的使用许可给实体。

业务合作伙伴可能包括与实体有客户或供应商关系的关联组织。业务合作伙伴需要有关实体系统及其内部控制的信息来管理和评估与实体开展业务相关的风险。

预期使用者还可能包括实体人员、为实体客户和业务合作伙伴提供服务的执业人员以及具备足够知识和理解的监管机构。

除本文件所述以外的其他方也可能具备所需知识和理解。例如，潜在客户和业务合作伙伴可能在执行其供应商选择流程或评估供应商是否符合监管要求时获得了此类知识。此外，由业务客户或业务合作伙伴组成的非监管、标准制定机构代表其成员（例如行业联盟）也可能具备所需知识。如果他们具备所需知识，潜在客户和业务合作伙伴以及非监管标准制定机构可能是报告的预期使用者。

如前所述，SOC for Supply Chain 报告的设计是为了满足本节所述的预期使用者的共同信息需求。然而，这并不妨碍执业人员将执业人员报告的使用限制在更小的预期使用者子集。

## SOC for Supply Chain 检查概述

执业人员根据美国注册会计师协会的鉴证标准执行 SOC for Supply Chain 检查。这些标准为检查的执行和报告建立了要求。根据这些标准，鉴证检查以这样一个概念为前提，即除执业人员之外的另一方（责任方）就主题事项是否根据合适的标准进行计量或评价作出断言。断言是关于主题事项是否符合标准或基于标准的任何声明或一组声明。

实体管理层通常是责任方。然而，在某些情况下，可能存在其他责任方。作为责任方，实体管理层准备包含在 SOC for Supply Chain 报告中的实体系统描述。此外，执业人员应要求责任方提供关于主题事项根据标准进行计量或评价的书面断言。管理层的书面断言包含在 SOC for Supply Chain 报告中，说明（a）实体系统的描述是否根据描述标准呈现，以及（b）描述中所述的控制措施，这些控制措施对于合理保证实体在整个期间内实现了其主要系统目标是必要的，是否基于适用的受托服务标准而有效。

执业人员设计并执行程序，以获取充分适当的证据来支持关于（a）描述是否呈现



the system that was designed and implemented in accordance with the description criteria and (b) the controls stated in the description, which are necessary to provide reasonable assurance that the entity achieved its principal system objectives, were effective during the period based on the applicable trust services criteria, in all material respects. As discussed in the subsequent section, the practitioner also presents, in a separate section of the report, a description of the practitioner's tests of controls and the results thereof.

## Contents of the SOC for Supply Chain Report

A SOC for Supply Chain examination results in the issuance of a SOC for Supply Chain report. The SOC for Supply Chain report includes four key components:

- 1. Entity management's description of the system the entity uses to produce, manufacture, or distribute products in accordance with the description criteria
- 2. Entity management's assertion about whether, in all material respects,
  a. the description of the entity's system is presented in accordance with the description criteria and
  b. the controls stated in the description, which are necessary to provide reasonable assurance that the entity achieved its principal system objectives, were effective throughout the period, based on the applicable trust services criteria
- 3. The practitioner's opinion about whether, in all material respects,
  a. the description of the entity's system is presented in accordance with the description criteria and
  b. the controls stated in the description, which are necessary to provide reasonable assurance that the entity achieved its principal system objectives, were effective throughout the period, based on the applicable trust services criteria
- 4. The practitioner's description of the procedures performed and the results thereof

## Description Criteria for Preparation of the Description of an Entity’s System

DC section 300, 2020 *Description Criteria for a Description of an Entity's Production, Manufacturing, or Distribution System in a SOC for Supply Chain Report*, includes the description criteria to be used by entity management to prepare and evaluate the description of the entity's system. Supplement A presents an excerpt from that document.

Applying the description criteria in actual situations requires judgment. Therefore, implementation guidance is included for each criterion. The implementation guidance presents factors to consider when making judgments about the nature and extent of disclosures called for by each criterion. The implementation guidance does not address all possible situations; therefore, management

> ³ All DC sections can be found in AICPA *Description Criteria*.

needs to consider the facts and circumstances of the entity and its environment when applying the description criteria.

## The Trust Services Criteria for Evaluation of Control Effectiveness

TSP section 100, *2017 Trust Services Criteria for Security, Availability, Processing Integrity, Confidentiality, and Privacy*, (the 2017 trust services criteria)⁴ includes the control criteria used to evaluate the effectiveness of controls relevant to the trust services category or categories included within the scope of a specific examination. Supplement B presents an excerpt from that document.

Because applying the trust services criteria requires judgment, that document also includes points of focus for each criterion. The Committee of Sponsoring Organizations of the Treadway Commission's 2013 *Internal Control — Integrated Framework* (COSO framework) states that points of focus represent important characteristics of the criteria in that framework.⁵ Consistent with the COSO framework, the points of focus may assist management when designing, implementing, and operating controls over security, availability, processing integrity, confidentiality, and privacy. In addition, the points of focus may assist entity management when evaluating whether controls stated in the description, which are necessary to provide reasonable assurance that the entity achieved its principal system objectives, were effective based on the applicable trust services criteria. The trust services criteria are discussed in more detail later in this appendix.

## The Entity’s System Objectives and Principal System Objectives

An entity adopts a mission and vision, sets strategies, and establishes objectives to help it meet its mission and vision based on its strategies. Management designs and implements individual production, manufacturing, or distribution systems to achieve certain specific objectives (referred to as *system objectives*) and controls within the system to mitigate the risks that would prevent the entity from achieving those objectives.

A SOC for Supply Chain examination addresses the system objectives that could reasonably be expected to influence the relevant decisions of the intended users. These system objectives, referred to as *principal system objectives*, typically address the category or categories addressed by the examination and relate to achieving commitments, specifications, or requirements. Management discloses its principal system objectives in the system description. When evaluating the effectiveness of controls, management makes that evaluation in light of what the system controls were designed to achieve; the principal system objectives.

As discussed earlier, an entity's principal system objectives are those that could reasonably be expected to influence relevant decisions made by intended users.

⁴ All TSP sections can be found in AICPA *Trust Services Criteria*.
⁵ ©2020 Committee of Sponsoring Organizations of the Treadway Commission (COSO). All rights reserved.

Typically, an entity's principal system objectives relate to achieving commitments, specifications, or requirements. An entity's commitments often relate to meeting the product's specifications and meeting other production, manufacturing, and distribution specifications. Commitments may also relate to other matters (for example, conforming with a variety of other standards and criteria such as the risk entity management framework issued by the National Institute of Standards and Technology [NIST], the cybersecurity standards issued by the International Organization on Standardization [ISO], or the Food and Drug Administration regulations in Code of Federal Regulations (CFR), *Electronic Records; Electronic Signatures*, Title 21, Part 11). An entity may also make commitments about different aspects of the product or its distribution, including commitments related to a product's performance specifications and availability.

## Table 1

### Types of Matters Addressed by Principal System Objectives
by Trust Services Category

| Trust Services Category | Matters That Might Be Addressed by the Entity’s Principal System Objectives |
|---|---|
| **Security** | Commitments regarding the protection of the system from physical and logical (including cybersecurity) risks |
| **Availability** | The product's availability in the quantities and at the times agreed on with customers |
|  | The achievement of delivery commitments made to customers, including the timing of delivery, storage and transportation commitments, and the system requirements necessary to achieve those commitments (for example, commitments made to a pharmaceutical company related to the maintenance of products at specific temperatures during the distribution process) |
|  | Distribution of the product in accordance with applicable laws and regulations regarding timing, storage, and transportation |
| **Processing Integrity (Over the Provision of Services or the Production, Manufacturing, or Distribution of Goods)** | The system's ability to produce products that achieve product performance specifications (the physical characteristics or functionality of a product) |





## 受托服务类别下由主要系统目标处理的事项类型 — *续*

| 受托服务类别 | 实体主要系统目标可能处理的事项 |
| :--- | :--- |
|  | 系统实现对客户作出的其他承诺的能力 |
|  | 系统符合实体为满足或遵守法律法规、行业标准或客户要求而制定的生产要求的情况（例如，制造商可能在合同上要求在生产过程中执行某些行业标准的质量控制测试） |
| 保密性 | 实现对客户或业务合作伙伴作出的具体承诺（例如，对业务合作伙伴关于实体在生产过程中使用其知识产权的承诺） |
| 隐私 | 实现实体隐私声明或隐私政策中确定的承诺和系统要求 |

实体管理层负责设计、实施和运行系统及相关控制，以基于适用的受托服务类别，对实现其主要系统目标提供合理保证。它还负责在描述中披露实体的主要系统目标，以便预期使用者能够理解系统如何运作以及实体管理层和执业人员如何评估控制的有效性。

## SOC 供应链审查未处理的事项

如前一节所述，审查可能涉及一个或多个受托服务类别。当审查涉及处理完整性时，执业人员的意见涉及系统控制是否有效以提供合理保证，所生产或制造的货物符合其产品性能规范。

然而，执业人员的意见并不涉及系统所生产的货物是否无缺陷或是否会按设计运行。换句话说，执业人员的意见并不是*保证或担保*所生产的货物将符合产品性能规范或其他对客户的承诺。因此，执业人员的意见不涉及产品是否适合用途或可销售。

## 委托执业人员前实体管理层的责任

实体管理层负责对系统及相关控制提供合理保证以实现其所设定的系统目标，具有合理的依据。管理层信念的基础部分基于管理层已：

- a. 确定了主要系统目标。
- b. 识别并分析了威胁实现这些主要系统目标的风险。
- c. 设计、实施和运行必要的控制，以基于适用的受托服务准则，提供合理保证主要系统目标得以实现。

在委托执业人员执行审查之前，实体管理层负责做出影响审查范围的各种决策，包括以下内容：

- a. 识别审查所涉及的实体所生产、制造或分销的货物
- b. 识别待审查系统的组成部分
- c. 识别该系统的边界
- d. 如果审查旨在满足业务合作伙伴以及客户的需求，识别因实体对提供用于生产或制造系统之知识产权或组件的业务合作伙伴的责任而产生的风险（如有）
- e. 选择审查所涉及的受托服务类别或类别
- f. 确定审查所涉及的时间段

在实体管理层能够履行这些责任之前，实体管理层可能需要执业人员对某些事项进行澄清。例如，实体管理层可能对某些流程或组件是否被视为待审查系统的一部分，或供应商的控制对实体实现其主要系统目标的能力产生的影响有疑问。

## 定义待审查的系统

SOC 供应链业务的主题事项围绕实体所设计、实施和运行的系统及相关控制，以制造、生产或分销货物。审查在以下方面具有灵活性：

- 实体用于生产、制造或分销有形产品（例如，飞机发动机）或无形产品（例如，商业现成应用）的系统和控制
- 实体用于运营生产线的所有系统和控制
- 实体用于生产、制造或分销特定设施或工厂内生产或制造的货物所使用的所有系统和控制

可能存在实体管理层尚未准备好就实体系统内的控制是否有效实现实体主要系统目标作出断言的情况。在这种情况下，而不是就控制是否有效实现实体主要系统目标作出断言，实体管理层仅就已实施控制的设计适当性作出断言。对此类断言的审查称为仅设计审查，包括考虑以下事项：（1）实体的系统描述是否按照描述标准呈现，以及（2）描述中所述的已实施控制是否在控制有效运行的情况下，设计适当以实现实体的主要系统目标。仅设计审查可能对希望了解实体系统及其为实现主要系统目标而实施的控制的预期使用者有用。然而，它不会为预期使用者提供足够的信息来评估实体系统内控制的有效性。

实体管理层负责识别待审查的具体主题事项，包括识别系统的组成部分和待审查系统的边界。管理层还负责建立其主要系统目标并选择审查所涉及的受托服务类别或类别，以及选择待审查的时间段。以下段落简要概述了这些因素中的每一个以及它们如何影响业务的主题事项。

系统被定义为由人员设计、实施和运行的基础设施、软件、程序和数据，以根据管理层指定的要求实现组织的一个或多个特定目标（例如，涉及货物生产或交付的目标）。系统组件可分为以下五类：（1）基础设施，（2）软件，（3）人员，（4）数据，（5）程序。例如，对于制造或生产系统，基础设施包括制造系统的组件以及其运行的流程。虽然投入物，例如原材料，并不是系统的组成部分，但它们通常是产品得以生产或制造所必需的。因此，原材料和其他投入物（例如，采购的组件）通常在生产或制造过程中很重要，因此在描述中除了系统的组成部分外还会披露。

确定超出待审查系统边界的功能或流程，并在描述中说明它们，通常是必要的，以防止预期使用者对系统描述和执业人员意见产生误解。因此，如果存在使用者可能对特定功能或流程是否属于待审查系统产生混淆的风险，描述需要澄清哪些流程或功能在审查范围内，哪些不在。

例如，实体以下功能或流程可能超出待审查系统的边界，因为它们不太可能


与实现安全、可用性、处理完整性、保密性和隐私方面的主要系统目标相关：

-   该实体用于为所制造产品向客户开具发票的过程
-   用于收集和报告不直接影响成品的可持续发展事项的过程

在涉及安全性和可用性的检查中，例如，系统边界通常会至少涵盖与生产或制造过程相关的所有系统组件，从原材料或组件的接收，到商品的生产，再到将成品转移给第三方物流公司以分发给客户或直接转移给客户。系统边界通常不包括辅助功能，例如将生产信息与实体内部用于次要目的的其他信息相结合（例如客户指标跟踪和财务报告系统）。

当检查涉及处理完整性，且执业者认为产品本身可能无法满足其产品性能规范时，执业者可能需要考虑系统设计的适当性。在这种情况下，被检查的系统可能需要包括与系统能否生产符合其产品性能规范的产品相关的系统设计控制。如果系统是在检查期开始之前的一段时期内设计的，执业者可以考虑检查期内运行的控制（例如客户投诉控制以及用于监控产品设计缺陷的其他控制）是否能有效识别系统设计适当性方面的缺陷。

如果嵌入式逻辑是产品的一部分（例如CPU芯片中的微码），该逻辑由被排除在系统之外的供应商提供，且描述中未明确说明嵌入式逻辑的生产由该供应商负责，因此不在系统边界之内，则执业者可能认为缺少此类信息属于错报。在这种情况下，执业者评估该错报是否对报告使用者的决策构成重大影响，并会导致描述产生误导。

在涉及保密性或隐私的检查中，系统边界将至少涵盖与机密或个人信息生命周期相关的所有系统组件，该生命周期包括通过明确定义的流程和非正式临时程序对机密或个人信息进行收集、使用、保留、披露以及处置或匿名化。

## 选择检查所涵盖的信任服务类别

除了识别系统组件外，还需要考虑检查将涵盖哪些信任服务类别。如前所述，信任服务标准用于衡量供应链SOC检查中控制的有效性，检查可以涵盖安全、可用性、处理完整性、保密性或隐私中的任何一个或所有信任服务类别。

在大多数情况下，检查将涵盖最符合预期用户信息需求的类别。通常，所涵盖的类别是通过考虑实体向其客户和业务伙伴作出的承诺来确定的。

由于对技术的依赖增加以及对网络安全风险的担忧，在大多数使用信任服务标准进行的检查中，安全很可能会被涵盖。通常，实体的客户和业务伙伴也对可用性控制的有效性感兴趣，因为此类控制可能对其履行承诺至关重要。例如，依赖该实体制造的安全气囊的客户，很可能希望了解该实体为实现其向客户作出的可用性承诺而设计、实施和运行的流程和控制。因此，同时涵盖安全和可用性的供应链SOC检查很可能满足预期用户作为一个群体的信息需求。

在某些情况下，预期用户可能还对实体用于生产、制造或分销货物的系统的处理完整性感兴趣，包括该系统各组件的处理完整性（例如硬件、工装、软件和信息）。处理完整性关注的是系统控制，这些控制可降低因生产过程失败而导致实体系统目标无法实现的风险。假设产品包含实现实体一项或多项主要系统目标所必需的嵌入式逻辑（例如嵌入式计算机的固件），并且该嵌入式逻辑是实体向其客户和业务伙伴作出的持续服务承诺的对象。在这种情况下，预期用户可能对实体为达成系统处理完整性而设计、实施和运行的流程和控制感兴趣，其中包括属于产品本身一部分的生产系统组件（例如CPU芯片中的微码）。因此，除了安全和可用性之外，还涵盖处理完整性的检查可能最能满足这些预期用户的需求。

当实体在生产过程中使用专有客户信息或个人信息时，用户也可能对这些信息的控制感兴趣。在这种情况下，同时涵盖保密性或隐私的检查可能最能满足用户的需求。

## 信任服务标准的类别

信任服务标准涉及以下五个类别：

-   a. *安全*。信息和系统受到保护，防止未经授权的访问、未经授权的信息披露以及对系统造成损害，这些损害可能危及信息或系统的可用性、完整性、保密性和隐私，并影响实体实现其目标的能力。
-   b. *可用性*。信息和系统可用于运行和使用，以实现实体的目标。
-   c. *处理完整性*（*涉及服务提供或商品的生产、制造或分销*）。系统处理完整、有效、准确、及时且经授权，以满足实体的目标。（在供应链SOC检查中，术语*处理完整性*涉及生产处理。换言之，生产处理完整、有效、准确、及时且经授权，以生产、制造或分销符合实体规格的商品。）
-   d. 保密性。被指定为机密的信息受到保护，以实现实体的目标。
-   e. 隐私。个人信息的收集、使用、保留、披露和处置均以实现实体的目标为前提。

根据检查范围内包含的类别，适用的信任服务标准包括

-   a. 所有五个信任服务类别共有的标准（通用标准），以及
-   b. 与可用性、处理完整性、保密性或隐私类别相关的附加具体控制活动标准。

例如，如果检查仅涵盖可用性，则测试的控制将是涵盖所有通用标准和可用性标准的控制。

通用标准提供了用于解决控制环境（CC1系列）、沟通与信息（CC2系列）、风险评估（CC3系列）、控制监控（CC4系列）以及与控制设计和实施相关的控制活动（CC5系列）的具体标准。这些标准是COSO框架中阐述的内部控制原则。除COSO原则外，通用标准还补充了针对逻辑和物理访问（CC6系列）、系统操作（CC7系列）、变更管理（CC8系列）和风险缓解（CC9系列）的一般IT控制的控制活动的具体标准。

AICPA的鉴证服务执行委员会已确定，通用标准适用于评估控制的有效性，这些控制对于合理保证实体实现其与安全相关的主要系统目标是必要的；无需额外的控制活动标准。对于可用性、处理完整性、保密性和隐私类别，一套完整的标准包括：(a)通用标准和(b)适用于特定类别的控制活动标准。表2列出了在评估每个信任服务类别的控制有效性时需要处理的信任服务标准。

## 表2

## 评估控制设计和运行有效性的标准

| 信任服务类别 | 通用标准 | 附加类别特定标准 |
|---|---|---|
| 安全 | X |  |
| 可用性 | X | X（A系列） |
| 处理完整性（涉及服务提供或商品的生产、制造或分销） | X | X（PI系列） |
| 保密性 | X | X（C系列） |
| 隐私 | X | X（P系列） |




基于信任服务准则 CC6.2 的客户员工身份凭证，该准则规定：“在向用户发放系统凭证并授予系统访问权限之前，实体必须注册并授权新的内部和外部用户，其访问权限由该实体管理。对于由实体管理其访问权限的用户，当用户访问权限不再被授权时，其系统凭证将被移除。”

如果某项客户职责不属于合同性确认性控制（CCC），通常不会在描述中披露；相反，它通常通过产品文档或用户手册进行沟通。然而，实体管理层通常会在描述中识别其向外部方进行的沟通类型。

当实体管理层仅向特定的预期用户（例如在与客户的合同中）传达客户职责时，实体管理层应考虑其他潜在用户是否可能误解该报告。在这种情况下，可能需要将报告的使用限制在指定的用户范围内。如果实体管理层不打算将报告的使用限制在预期用户的一个子集内，实体管理层将在描述中识别重大的客户职责，以便预期用户能够理解主题事项和相关从业者的意见。在这种情况下，报告将适合预期用户使用。

## 识别供应商和补充供应商控制

### 实体实现其主要系统目标所必需控制功能的供应商

生产、制造或分发产品的实体会从供应商处获取原材料、组件或其他货物（例如生产设备）。它还可能将各种处理功能（例如 IT 网络的提供）外包给服务提供商。在本附录中，“供应商”一词用于指代向实体提供产品（如原材料、组件或其他货物）或服务（及其员工）的供应商、经销商和服务提供商。供应商可以是实体的外部独立实体，也可以是相关实体，例如拥有该实体的同一公司的子公司。

在大多数情况下，实体很可能对关键供应商提供的产品和服务质量拥有有效的控制，从而为其实现主要系统目标提供合理保证。如果情况属实，预期用户可能无需了解供应商的控制，因为这些控制对于实体实现其主要系统目标并非必不可少。实体可能通过以下情况对供应商的货物或服务拥有有效控制，从而实现其主要系统目标的例子包括：

- 实体拥有针对系统所有投入的健全质量检验流程，该流程在收到供应商货物时执行。该流程包括检查运输容器和包装箱是否有物理损坏，统计抽样选择投入样本以对照投入规格和要求进行测量，以及由生产人员对投入进行目视检查。在这种情况下，实体的控制足以降低投入不符合规格的风险。

- 实体对供应商用于生产新软件的系统拥有健全的控制（包括变更管理控制），随后实体在其生产流程中使用该软件。在这种情况下，实体的第三方风险和控制的评估活动足以使实体实现其主要系统目标。

- 在一项针对可用性的检查中，供应商负责每季度对实体的备用电源系统进行维护。如果实体实施了针对供应商控制的自有第三方风险和控制的评估活动，则供应商的控制对于实体实现其主要系统目标并非必不可少。

- 实体将应用开发测试外包给供应商，并在供应商合同中规定，供应商有责任执行实体认为对缓解与供应商开展业务相关风险所必需的控制。实体指定一名员工监督外包服务，该员工将供应商的测试计划、测试脚本和测试数据与实体的应用变更请求和详细设计文档进行对比。被指定的实体员工还在供应商的应用程序变更获得供应商批准并提交给实体进行用户验收测试之前，审查供应商执行的测试结果。基于适用的信任服务准则，供应商的控制对于实体断言其控制提供了合理保证（即实体实现其主要可用性承诺）可能并非必不可少。

### 补充供应商控制

然而，在其他情况下，实体可能会将某些职责委托给供应商，并期望供应商对产生或交付所接收货物和服务的流程执行特定的控制。因此，有效的供应商控制可能是实体实现其主要系统目标所必需的。例如，实体可能无法监控基础设施即服务提供商（IaaS）中旨在维护业务伙伴专有信息机密性的控制。在另一个例子中，原始设备制造商客户可能指定实体需要从特定客户批准的制造商处获取特定组件，并且实体应“原样”接受该组件。

当供应商执行的控制与实体的控制相结合，对于实现实体的主要系统目标是必需的时，这些控制被称为补充供应商控制（CSC）。鉴于 CSC 对报告用户的重要性，它们在描述中予以披露。呈现 CSC 的最常见方法是通过仅包括由实体负责执行的流程和控制来定义系统，并在描述中识别实体期望供应商实施的 CSC。这被称为剔除法。

在使用剔除法时，描述将识别假设供应商已实施的 CSC 类型以及受其影响的信任服务准则。CSC 通常以表格形式呈现在描述的末尾，并列出每个 CSC 相关的信任服务准则。实体管理层在确定如何在描述中呈现 CSC 时，可以请求从业者的协助。从业者可以提供其他实体进行的 CSC 披露示例，并可以就改进描述中 CSC 的呈现方式提出建议。

### 使用包容法

在某些情况下，由于供应商在生产、制造或分发过程中的角色重要性，实体管理层可能会选择在描述中呈现供应商的相关流程和控制。这种呈现方法被称为包容法。在包容法下，实体系统的描述包括与从供应商获得的货物或服务性质以及供应商系统组件（包括相关控制）有关的一些披露。在使用包容法时，供应商的系统组件和控制将受到从业者检查程序的约束。

当使用包容法时，供应商管理层也是检查中的责任方，并必须履行与实体管理层相同的许多要求，包括在检查结束时向从业者提供书面声明和承诺函。因此，使用包容法涉及从业者、实体管理层和供应商之间的广泛计划和沟通。

当实体使用多个供应商，且其控制与实体的控制相结合，对于实体提供合理保证（即实现其主要系统目标）是必需的时，使用包容法变得更加复杂。当多个供应商的控制都可能与报告用户相关时，实体管理层可能会选择对一个或多个供应商使用包容法，而对其他供应商使用剔除法。在这些情况下，描述需要明确说明哪些供应商及相关功能和流程包含在检查范围内，哪些被剔除。

由于使用包容法涉及的额外复杂性，实体和供应商管理层可能会在业务承接期间就使用包容法达成一致。此外，为了促进流程，实体管理层通常会与供应商管理层协调包容法的使用。如果使用包容法，需要达成一致或协调的事项包括：

- a. 检查的范围以及从业者报告覆盖的期间
- b. 供应商管理层的确认，即其将向从业者提供书面声明和承诺函
- c. 包容性描述的计划内容和格式
- d. 供应商和实体的代表，以及谁将负责
    - i. 提供各实体的描述
    - ii. 整合描述
- e. 控制测试的时间安排

在规划期间，从业者可能会与实体管理层讨论这些事项。此时，从业者也可以讨论他或她



believes it will be possible to obtain evidence that supports the portion of the opinion that relates to the supplier's controls.

The practitioner determines whether supplier management will provide a written assertion and representation letter. In addition, the practitioner determines whether it will be possible to obtain evidence that supports the portion of the opinion that addresses the supplier. If entity management wishes to use the inclusive method, but supplier management refuses to provide a written assertion, the entity will not be able to use the inclusive method.

In addition to providing the practitioner with a written assertion and representation letter at the end of the examination, supplier management is also responsible for preparing a description of the supplier's system, including the completeness, accuracy, and method of presentation of the description. Entity management is responsible for evaluating the description of the supplier's system, as well as its own.

As a responsible party, supplier management is also responsible for the following:

- a. Designing, documenting, implementing, and operating controls that are suitably designed and operating effectively
- b. Having a reasonable basis for its assertion
- c. Providing the practitioner with written representations at the end of the engagement
- d. If the practitioner plans to use internal auditors to provide direct assistance, providing the practitioner with written acknowledgment that internal auditors providing direct assistance to the practitioner will be allowed to follow the practitioner's instructions and that the supplier will not intervene in the work the internal auditors perform for the practitioner
- e. Providing the practitioner with the following:
  - i. Access to all information, such as records, documentation, service level agreements, and internal audit or other reports, that supplier management is aware of and that is relevant to the description of the supplier's system and assertion
  - ii. Access to additional information that the practitioner may request from supplier management for the examination
  - iii. Unrestricted access to supplier personnel from whom the practitioner determines it is necessary to obtain evidence relevant to the examination
- f. Disclosing to the practitioner the following:
  - i. Incidents (that are clearly not trivial) on the part of the supplier or its employees of noncompliance with laws and regulations, fraud, or uncorrected misstatements related to the system or goods produced, manufactured, or distributed, and whether such incidents have been communicated appropriately to affected parties
  - ii. Incidents (that are clearly not trivial) at its suppliers or business partners of noncompliance with laws and regulations, fraud, or uncorrected misstatements related to the
  - iii. Knowledge of any actual, suspected, or alleged intentional acts that could adversely affect the description of the supplier's system or the effectiveness of controls
  - iv. Any deficiencies in the design of controls of which supplier management is aware
  - v. All instances in which controls have not operated as described
  - vi. All identified system incidents that resulted in a significant impairment of the supplier's commitments to the entity and its customers during the period of time covered by the description
  - vii. Any events subsequent to the period covered by the description of the system, up to the date of the practitioner's report, that could have a significant effect on the description, the design or operation of the controls, or management's assertion

Supplier management's assertion ordinarily would be expected to address the same matters addressed by entity management in its assertion, including the effectiveness of controls and whether the description presents the system the supplier uses to produce, manufacture, or distribute products to the entity and its customers in accordance with the description criteria. However, in some cases, entity management might design the controls for the supplier. When entity management designs the controls for the supplier, entity management takes responsibility for the suitability of the design of its own controls and the supplier's controls; therefore, the supplier's assertion may be limited to whether the description presents the system that it uses to produce, manufacture, or distribute products to the entity and its customers in accordance with the description criteria and whether the controls at the supplier operated as described.

## Agreeing on the Terms of the Engagement

The attestation standards require the practitioner to agree on and document the terms of the engagement with the engaging party. A written agreement, such as an engagement letter, reduces the risk that either the practitioner or entity management may misinterpret the needs or expectations of the other party. For example, it reduces the risk that entity management may rely on the practitioner to protect the entity against certain risks or to perform certain management functions.

The agreed-upon terms of the engagement should include the following:

- a. The objective and scope of the engagement
- b. The responsibilities of the practitioner
- c. A statement that the engagement will be conducted in accordance with attestation standards established by the American Institute of Certified Public Accountants
- d. The responsibilities of the responsible party and the engaging party, if different
- e. A statement about the inherent limitations of an examination engagement
- f. Identification of the criteria for the measurement, evaluation, or disclosure of the subject matter
- g. An acknowledgment that the engaging party agrees to provide the practitioner with a representation letter at the conclusion of the engagement

If the practitioner plans to use internal auditors to provide direct assistance, prior to doing so, the practitioner may also obtain written acknowledgment from the responsible party (usually entity management) that internal auditors providing direct assistance to the practitioner will be allowed to follow the practitioner's instructions and that the responsible party will not intervene in the work the internal auditors perform for the practitioner. If the engaging party is the responsible party, the practitioner may wish to include this matter in the engagement letter. In addition to the matters discussed in the previous two paragraphs, the practitioner may also want to reach an understanding with entity management about other matters, such as a draft of principal system objectives identified by entity management.

Although not required by the attestation standards, the practitioner would ordinarily request a signed engagement letter from the engaging party. The engaging party's refusal to provide the practitioner with an engagement letter may cause the practitioner to question whether a mutual understanding regarding the terms of the engagement has been reached and, in turn, may affect the practitioner's decision about whether to accept or continue the engagement.

## Entity Management Responsibilities During the Examination

In a SOC for Supply Chain engagement, entity management is responsible for the following:

- a. Preparing a description of the entity's system in accordance with the description criteria
- b. Providing a written assertion that accompanies the description of the entity's system, both of which will be provided to report users
- c. Having a reasonable basis for its assertion
- d. If the practitioner plans to use internal auditors to provide direct assistance, providing the practitioner with written acknowledgment that internal auditors providing direct assistance to the practitioner will be allowed to follow the practitioner's instructions and that the entity will not intervene in the work the internal auditors perform for the practitioner
- e. Providing the practitioner with the following:
  - i. Access to all information, such as records, documentation, service level agreements, and internal audit or other reports, that management is aware of and that are relevant to the description of the entity's system and assertion
  - ii. Access to additional information that the practitioner may request from management





- iii. 允许执业者不受限制地接触其认为有必要获取与 SOC 供应链鉴证相关的证据的实体内部人员。
- f. 向执业者披露以下内容：
  - i. 实体或其员工在与系统或生产、制造或分销商品相关的法律法规不合规、欺诈或未更正错报方面的事故（显然并非微不足道的事故），以及此类事故是否已适当通知受影响方
  - ii. 实体所知道的供应商或业务合作伙伴在与生产、制造或分销商品或相关系统相关的法律法规不合规、欺诈或未更正错报方面的事故（显然并非微不足道的事故）
  - iii. 知道任何可能对企业系统描述的呈现或控制有效性产生不利影响的实际、涉嫌或被指控的故意行为
  - iv. 其知道的任何控制设计缺陷
  - v. 控制未按描述运行的所有实例
  - vi. 在描述所涵盖期间内，导致实体达成其主要系统目标受到重大损害的所有已识别系统事故

在执业者要求下，实体管理层可在业务约定书或其他合适的书面沟通形式中确认这些责任。

## 编制服务组织的系统描述

如前所述，按照描述标准编制的实体系统描述旨在使客户、业务伙伴及其他人员更好地理解实体的系统。除其他事项外，该描述提供了关于威胁实体达成其主要系统目标的风险以及实体为管理这些风险而实施的程序和控制措施的信息。例如，关于实体生产、制造或分销的产品类型；生产、制造或分销过程的特征；实体运营的技术环境；以及生产、制造或分销实体产品的系统的组成部分的披露，使用户能够更好地理解系统控制运作的环境。

实体管理层负责编制根据附录 A“SOC 供应链报告中实体生产、制造或分销系统描述的 2020 年描述标准”中呈现的描述标准设计和实施的系统描述。执业者负责确定描述是否按照描述标准编制。

通常，实体管理层基于支持系统和系统操作的文档以及考虑系统内的政策、流程和程序（控制措施）来编制描述。描述没有规定格式。实体管理层可以多种方式组织描述，前提是包含描述标准所要求的披露。流程图、矩阵、表格、图表、上下文图或它们的组合可用于补充描述中包含的叙述。

描述中包含的披露程度可能因实体及其活动的规模和复杂程度而异。此外，描述无需涵盖实体生产、制造或分销商品系统的各个方面，特别是如果系统的某些方面与预期使用者不相关或超出了 SOC 供应链鉴证的范围。例如，关于实体与向制造产品客户开具账单相关过程的披露，不太可能与报告使用者相关。类似地，虽然描述可以涵盖通过手动和自动化系统生产、制造或分销产品的程序，但无需披露这些过程中的每一个步骤。

然而，预期披露应以能够合理满足预期使用者共同信息需求的详细程度进行。例如，假设某实体使用供应商提供其计算机处理基础设施，并在描述中包含以下披露：

> 该实体将其计算机处理的某些方面外包给 Computer Outsourcing Associates。

在这种情况下，实体管理层可能考虑是否更广泛的披露（例如以下披露）可能更有用：

> 该实体在 Computer Outsourcing Associates 托管其 ERP 系统。
> 该实体保留应用程序变更和用户访问的责任，而 Computer Outsourcing Associates 提供计算机处理基础设施及其变更。

尽管预期描述应包含针对每个描述标准的相关披露，但此类披露并不旨在达到如此详细的程度，以至于可能增加敌对方利用安全漏洞的可能性，从而损害实体达成其主要系统目标的能力。相反，披露旨在使预期使用者理解实体面临的风险性质以及这些风险实现的潜在影响。

在评估描述是否按照描述标准呈现时，实体管理层和执业者可考虑以下事项：

### 表 3：可能表明描述是否符合描述标准的特征

| 可能表明描述符合描述标准的特征 | 可能表明描述不符合描述标准的特征 |
|---|---|
| 描述阐明了实体为生产、制造或分销产品而设计和实施（投入使用）的系统的重大方面。 | 描述声明或暗示某些信息技术组件存在，而实际上它们并不存在。 |
| 描述没有意外或故意遗漏或歪曲可能与预期使用者决策相关的信息。 | 描述声明或暗示某些流程和控制措施已实施，而实际上它们并未被执行。 |
| 在相关范围内，描述包含关于每个描述标准的信息，且未使用遗漏或歪曲信息的语言。 | 描述包含无法客观评估的陈述（例如，夸大宣传）。 |
| 描述以预期对作为群体的预期使用者有意义的详细程度编制。 | 描述编制得过于概括，遗漏了对预期使用者决策有重大意义的信息。 |
| 呈现的特征（如格式）是适当的，因为描述标准允许呈现方式的差异。 | 描述呈现的某些特征令人困惑，并掩盖了关于系统的关键信息。 |

在评估描述是否按照描述标准呈现时，实体管理层还可考虑每个标准的实施指南。实施指南提供了在判断每个标准所要求的披露性质和程度时应考虑的因素。实施指南并未涉及所有可能的情形；因此，实体管理层在应用描述标准时需要考虑实体及其环境的事实和情况。

在某些情况下，可能需要额外的披露来补充描述。实体管理层关于此类额外披露是否必要的决定，以及执业者对实体管理层决定的评估，涉及考虑这些披露是否可能影响与预期使用者决策相关的信息。





users. 可能需要额外披露的示例包括以下内容：

- a. 在检查的具体情况下应用描述标准时所做的重大解释（例如，什么构成安全事件或事故）
- b. 后续事件，根据其性质和重要性而定

如果从业者识别出描述中存在描述错报（包括遗漏），而从业者认为这些错报可能是重大的，从业者通常会要求实体管理层修改描述。在大多数情况下，实体管理层会做出适当的修改以使断言与意见保持一致。但是，如果实体管理层没有适当修改描述以解决从业者的关切，从业者将评估错报是否重大，并确定该错报对其对描述的意见的影响。

## 描述标准要求的披露

## DC1：与所生产、制造或分销的商品类型相关的披露

描述标准 DC1 要求实体管理层在描述中包含有关实体所生产、制造或分销的商品类型以及，如果相关，生产、制造或分销流程的特征的某些披露。描述标准 DC1 包含实施指南，旨在帮助从业者在评估有关所生产、制造或分销的商品类型的披露的性质和范围时提供帮助。

## DC2：关于实体主要系统目标的披露

如前所述，管理层负责设计、实施和运行系统及相关控制，以在基于适用的受托服务标准确定的主要系统目标方面获得合理保证。识别主要系统目标是实体风险评估过程的一部分，对于有效的内部控制系统是必要的。

预期使用者需要了解管理层确定的主要系统目标与受托服务标准之间的关系，以便了解控制有效性是如何评估的；因此，描述标准 DC2 要求实体管理层在描述中披露主要系统目标。实体的主要系统目标通常涉及系统满足产品性能规范、承诺和要求，以及生产、制造或分销承诺和要求的能力。主要系统目标是与描述所涉及的受托服务类别或类别相关且可能影响预期使用者报告相关决策的目标。

## DC3：关于系统事故的披露

描述标准 DC3 要求实体管理层在描述中包含与系统事故相关的某些信息，这些事故是由于控制无效或以其他方式导致一个或多个主要系统目标实现出现重大失败的结果。具体而言，描述预期包括有关每起事故的以下信息：

- a. 每起事故的性质
- b. 事故 surrounding 的时间
- c. 事故的范围（或影响）及其处理情况

描述标准 DC3 包含实施指南，旨在帮助从业者在评估关于系统事故的披露的性质和范围时提供帮助。以下是关于已识别系统事故的披露示例：

在评估期间，公司发生了一起事故，其中一名在线入侵者获得了用于存储和配置其小部件中使用的嵌入式软件的服务器的访问权限。通过供应商软件所使用的服务器中一个此前未知的操作系统漏洞，入侵者对要加载到小部件中的软件和配置参数进行了未经授权的更改。攻击在获得未经授权访问后大约 66 小时被检测到，并在检测后 5 天内得到补救。公司在 5 天期间停止了小部件的制造，并召回了从初始未经授权访问时间到事故补救期间加载了该软件的所有小部件。公司将所有召回和未发货小部件的序列号与制造记录进行核对，未发现例外。根据此次核对，管理层认为所有带有未经授权软件的小部件都已得到处理。所有这些小部件随后在受控条件下被销毁。

作为补救措施的一部分，公司从事故发生前的备份中重新安装了操作系统和应用程序，并应用了操作系统供应商提供的软件补丁。

## DC4：关于影响实体生产、制造或分销的重大风险的披露

描述标准 DC4 要求实体管理层在描述中包含与影响实体生产、制造或分销的重大风险相关的某些信息。描述标准 DC4 包含实施指南，旨在帮助从业者在评估关于重大风险的披露的性质和范围时提供帮助。

## DC5：关于系统输入和组成部分的披露

描述标准 DC5 要求实体管理层在描述中包含与系统输入（原材料和其他投入）以及生产、制造或分销产品的系统组成部分相关的某些信息。描述标准 DC5 还包含实施指南，旨在帮助从业者在评估关于系统输入和组成部分的披露的性质和范围时提供帮助。

## DC6：关于单个控制和适用的受托服务标准的披露

描述标准 DC6 要求披露适用的受托服务标准（即与描述所涉及的类别相关的标准）以及为合理保证实体的主要系统目标得以实现而必要的相关控制。例如，如果描述涉及可用性，实体管理层将提供其为解决受托服务标准中的通用标准和可用性的额外受托服务标准而实施的控制的信息。

描述预期仅披露已实施的控制。对于这些已实施的控制，描述提供了足够详细的控制信息，使预期使用者，特别是客户和业务合作伙伴，能够了解此类控制如何影响他们与实体的互动。表 4 提供了一般会包含在描述中的每个控制的信息。表中右栏中的粗体斜体表示专门回答左栏提出问题的文本。

### Table 4

### 包含在描述中的控制披露

| 包含在描述中的披露 | 说明性控制 |
|---|---|
| What: 控制适用的主题事项 | 对生产、源代码和目标代码的更改请求是通过准备并提交更改票据给变更控制委员会审批来发起的。系统会自动记录对生产、源代码和目标代码所做的更改。每周，变更经理都会审查系统更改日志和已批准的更改票据，以通过确定（1）日志中的每个条目都有已批准的更改票据，以及（2）已批准的更改票据中确定的所有更改都已记录在日志中，来识别未经授权和遗漏的更改。任何未经授权或遗漏的更改都会在事件管理系统中输入为事件记录。事件记录分配给受影响应用程序的应用程序经理进行后续处理和解决。变更经理跟踪未解决记录直至解决，并向应用开发副总裁准备每周报告。 |





## 描述中应包含的控制披露 — 继续

| 描述中应包含的披露 | 示例控制 |
|---|---|
| **Who:** 执行控制的责任方 | 对生产、源代码和目标代码的变更请求通过准备并提交变更单至变更控制委员会审批来发起。系统会自动记录对生产、源代码和目标代码所做的变更。每周，*变更经理* 会审查系统变更日志和已批准的变更单，以通过确定 (1) 日志中的每一项都有对应的已批准变更单以及 (2) 已批准变更单中识别出的所有变更均已记录在日志中，来识别未经授权和遗漏的变更。任何未经授权或遗漏的变更都会录入事件管理系统的事件记录。事件记录分配给受影响应用的应用经理以跟进和解决。*变更经理* 跟踪开放记录至解决，并向*应用开发副总裁* 准备每周报告。 |
| **How:** 执行控制所进行的活动性质，包括执行控制时使用的信息来源 | 对生产、源代码和目标代码的变更请求通过*准备并提交变更单*至变更控制委员会审批来发起。系统会自动记录对生产、源代码和目标代码所做的变更。每周，变更经理*审查系统变更日志和已批准的变更单，以通过确定 (1) 日志中的每一项都有对应的已批准变更单以及 (2) 已批准变更单中识别出的所有变更均已记录在日志中，来识别未经授权和遗漏的变更*。任何未经授权或遗漏的变更都会*录入事件管理系统的事件记录*。事件记录分配给受影响应用的应用经理*以跟进和解决*。变更经理*跟踪开放记录至解决并向*应用开发副总裁准备每周报告。 |

## 描述中应包含的控制披露 — 继续

| 描述中应包含的披露 | 示例控制 |
|---|---|
| When: 控制执行的频率，或其发生的时机 | 对生产、源代码和目标代码的变更请求通过准备并提交变更单至变更控制委员会审批来发起。系统会自动记录对生产、源代码和目标代码所做的变更。每周，变更经理审查系统变更日志和已批准的变更单，以通过确定 (1) 日志中的每一项都有对应的已批准变更单以及 (2) 已批准变更单中识别出的所有变更均已记录在日志中，来识别未经授权和遗漏的变更。任何未经授权或遗漏的变更都会录入事件管理系统的事件记录。事件记录分配给受影响应用的应用经理以跟进和解决。变更经理跟踪开放记录至解决，并向应用开发副总裁准备每周报告。 |

尽管实体管理层可能在描述中描述控制，但其也可能引用控制表（如前述表格）并将其呈现在报告的单独部分。如果描述引用了控制表，则该表被视为描述的一部分；因此，其也受到实务人员的检查范围。经常，实务人员在同一表格中描述所执行的控制测试及其结果。

实体可能拥有其认为超出描述所涉及系统边界的控制，例如与新客户向实体系统转换相关的控制。为避免报告使用者产生误解，预期描述应明确界定纳入检查范围的系统边界。

## DC7：关于互补客户控制和客户责任的披露

当互补客户控制（CCCs）与实体的控制相结合时，对于提供合理保证以实现实体的主要系统目标是必要的，描述准则 DC7 要求在描述中包含关于 CCCs 的某些披露。描述准则 DC7 还包含实施指南，旨在帮助实务人员在评估关于重大风险的披露性质和程度时提供帮助。

CCCs 通常由管理层在评估威胁其主要系统目标实现的风险时识别，并确定这些风险的缓解取决于由客户执行的控制。实体管理层通常会向实务人员提供其风险评估，其中识别了此类 CCCs。如果实体管理层的流程是非正式的，

实务人员可能需要执行额外的程序，例如包括 CCCs 的独立风险评估。

## DC8：与供应商相关的披露

当供应商执行的控制与实体的控制相结合以实现主要系统目标时，描述准则 DC8 要求在描述中包含关于供应商的某些披露；此类披露的性质取决于管理层为进行此类披露而选择的方法。

### 使用剔除法时的披露

当供应商执行的控制与实体的控制相结合以实现主要系统目标时，此类控制被称为互补供应商控制（CSCs）。在描述中呈现 CSCs 的最常见方法是仅包括其执行责任属于实体的流程和控制，并识别实体期望供应商实施的 CSCs。此方法称为剔除法。

使用剔除法时，DC8 要求披露以下信息：

- a. 供应商生产、制造或分销的原材料或组件的性质，或供应商提供的服务
- b. 供应商的控制旨在满足的每个适用的受托服务准则
- c. 实体管理层在设计实体系统时假设将由供应商实施且对于与实体控制相结合以提供合理保证实现实体主要系统目标而言是必要的 CSCs 类型

DC8 还包含实施指南，旨在帮助实务人员在剔除法使用时评估关于供应商的披露性质和程度。

为了对报告使用者有意义，描述中所述的 CSCs 预期包括那些特定于供应商生产、制造或分销的原材料或组件，或供应商提供的服务的控制。通常，实体管理层将 CSCs 呈现为供应商预期具备的控制的宽泛类别或控制类型。例如，实体可能识别以下 CSC 以解决受托服务准则 CC6.1，实体对受保护的信息资产实施逻辑访问安全软件、基础设施和架构，以保护其免受安全事件影响以满足实体的目标：

- 通过使用访问控制列表，原生操作系统和基于应用的安全性限制对系统基础设施的逻辑访问。

由于 CSCs 与实体的控制相结合，对于提供合理保证实现某些主要系统目标是必要的，因此重要的是描述中还应包括供应商实施这些控制的责任，并指出实体只有在整个期间内设计良好的 CSCs 有效的情况下才能实现相关的主要系统目标。CSCs 在评估控制设计的适当性时也会被考虑。




## 使用包含法时的披露

在某些情况下，实体管理层可能希望在其说明中列明供应商的相关流程和控制措施，以满足用户的共同信息需求，或因为供应商在实体的生产、制造或分销过程中具有重大意义。这种列报方式被称为包含法。根据包含法，供应商的基础设施、软件、人员、程序和数据的相关方面被视为实体系统的一部分；因此，它们会在说明中披露，并接受执业人员的审阅程序。说明中会分别识别实体层面的控制和供应商层面的控制。然而，对于区分这两者并没有规定的格式。

当使用包含法时，供应商管理层也是审阅中的责任方。由于使用包含法涉及额外的复杂性，实体管理层和供应商管理层通常在业务约定接受阶段就会就使用包含法达成一致。

当使用包含法来列报供应商提供的服务时，DC8 要求披露以下信息：

- a. 由该供应商生产、制造或分销的原材料或组件的性质，或该供应商提供的服务性质
- b. 系统中归属于供应商的部分
- c. 供应商的基础设施、软件、人员、程序和数据的 relevant aspects（相关方面）
- d. 供应商层面必要的控制措施，这些措施与实体的控制措施相结合，可为基于适用信任服务标准实现实体的主要系统目标提供合理保证

供应商控制的说明还可以包括供应商控制环境、风险评估流程、信息与沟通以及监控活动的方面，前提是与实体控制相关。说明应区分实体控制和供应商控制；然而，对此并没有规定的格式。

DC8 还包含实施指南，旨在协助执业人员在评估使用包含法时关于供应商的披露性质和范围。

## 适用于两种方法的考虑因素

无论管理层选择哪种方法，对实体系统的说明以及执业人员审阅的范围都应包括在实体设计、实施和运行的用于监控供应商控制有效性的控制的说明。这是因为对供应商的监控控制通常是内部控制系统的必要组成部分，用于提供合理保证，以确认实体的主要系统目标已实现。这些类型的控制基于信任服务标准 CC9.2 进行评估，该标准为：实体评估和管理与供应商和业务伙伴相关的风险。

## DC9：关于非相关标准的披露

描述标准 DC9 要求说明中披露任何与所描述系统不相关的特定适用信任服务标准及其不相关的原因。例如，如果某个适用的信任服务标准不适用于正在审阅的生产、制造或分销系统，则该标准可能不相关。

## DC10：关于期间内系统重大变化的披露

描述标准 DC10 要求披露在说明所涵盖的期间内，实体系统中与实现实体主要系统目标相关的重大变化。需要披露的重大变化包括那些可能对预期使用者相关的变化。

## 根据描述标准编制说明时的重大性考虑

如前所述，管理层会对说明是否呈现了按照描述标准设计和实施的系统做出断言。然而，说明中的错报可能由以下任何原因导致：

- 包含具有误导性或不恰当的信息（例如，掩盖描述标准要求的信息）
- 遗漏描述标准要求的信息（例如，信息不足或不完整）
- 在没有合理理由的情况下更改以前期间的披露
- 事实错报

由于系统说明是一种叙述性列报，考虑重大性涉及一些独特的判断。因此，在决定包含在说明中的披露性质和范围时，实体管理层需要考虑报表预期使用者的需求。

实体管理层在评估说明中包含的披露是否符合描述标准时，也可以考虑某些定性因素。此类因素的示例可能包括：

- 由于描述标准允许列报方式存在差异，因此为说明所采用的列报特征，如格式
- 关于描述标准要求披露的措辞选择是否省略或扭曲了信息
- 错报是故意行为的结果还是非故意的（例如，如果执业人员认为实体
- 考虑关于说明中所涉主题某些方面的信息是否可能对预期使用者更为重要。示例包括：
  - 关于替换安全气囊短缺的最新国家媒体报道，可能导致执业人员认为与供应商围绕其产品可用性相关控制的标准要求披露，对于最近特定车型安全气囊被召回的汽车制造商报表的预期使用者来说，比与供应商围绕保密性相关控制的标准要求披露更为重要。
  - 由于个人健康信息（PHI）经常成为黑客攻击的目标，当报表针对使用 PHI 的医疗设备制造商时，执业人员可能认为说明中与描述标准要求相关的安全性披露，比报表针对外科服装批发商时，对该报表预期使用者更为重要。

以下示例阐明了实体管理层在决定在实体系统说明中披露什么时的大体性考虑：

*示例 1。* 示例实体使用服务提供商来执行其后端职能，并选择使用排除法。说明中包含了关于服务提供商提供的服务性质的信息，并描述了实体就服务提供商执行的处理所执行的监控和其他控制。说明中包含这些信息，因为它们可能对预期使用者相关，因此此类信息将被视为实体系统说明中的重大事项。

*示例 2。* 示例实体在美国三个不同的工厂生产跑鞋：俄勒冈州库斯湾、伊利诺伊州内珀维尔和爱荷华州爱荷华城。俄勒冈和内珀维尔工厂使用相同的制造控制系统，1Run。爱荷华工厂使用一个独立的控制系统，Hawkeyes，该系统与 1Run 系统分割。总体而言，示例实体 80% 的鞋子在俄勒冈州制造，18% 在伊利诺伊州制造。爱荷华地区的工厂每年制造少于 5%，且过去三年平均仅占鞋子产量的 2%。因此，示例实体已确定俄勒冈和伊利诺伊工厂在报告范围内，并得出结论，爱荷华工厂对于跑鞋的整体制造并不具有重大性，因此不会包含在报告范围内。实体在报告中披露了这些细节，并指出 Hawkeyes 制造系统未包含在被审阅系统的边界内。




## 提供书面声明

鉴证准则要求执业者要求责任方提供一份书面声明，该声明需涉及业务约定主题。本指南中讨论的审查中，责任方的声明涉及 (a) 描述是否按照描述准则呈现了已设计并实施的系统，以及 (b) 描述中陈述的、为确保实体实现其主要系统目标所必需的控制措施，是否基于适用的信任服务准则被认为是有效的。

实体管理层的声明与描述及执业者的意见一同包含在供应链 SOC 报告中。由于管理层的声明在业务约定中起着重要作用，实体管理层可能会要求执业者在接受业务约定之前提供书面声明的示例。然而，实体管理层负责起草其书面声明，并可根据其惯例措辞该声明，只要该声明涉及管理层关于前述段落中 (a) 和 (b) 事项得出的结论即可。

## 对控制有效性评估拥有合理基础

除了关于描述是否按照描述准则呈现了已设计并实施的系统的声明外，实体管理层还必须就描述中陈述的、为确保实体实现其主要系统目标所必需的控制措施是否基于适用的信任服务准则被认为是有效的（控制有效性）提供声明。

为了就控制有效性做出声明，实体管理层必须为其声明拥有合理的基础。由于 (a) 控制的有效性与 (b) 监控控制之间的关系，执业者通常在业务约定接受之前与实体管理层讨论其声明的基础。这有助于确定该基础对于实体的规模和复杂性而言是否看似合理，以及执业者是否预计能够获取充分适当的证据以形成意见，这也是审查的先决条件。

实体管理层对其声明的基础通常严重依赖实体的监控活动。此类监控活动通常包括持续活动、独立评估或两者的结合。持续监控活动通常融入实体的正常常规活动中。监控活动尤为重要，因为实体经常与客户、业务伙伴、供应商以及其他可访问实体系统或代表实体传输信息或在他们与实体之间往返传输信息的各方进行互动。

如果第三方的控制措施与实体的控制措施相结合，对于提供合理保证以确保实体实现其主要系统目标是必要的，实体管理层将识别并评估与这些第三方开展业务的风险。如果这些风险对实现实体的主要系统目标构成重大威胁（例如，由于这些方访问系统的性质、关键原材料或组件的供应，或其代表实体运营的控制措施的性质……描述中陈述的控制措施是否有效。实体管理层的书面声明通常预期通过反映相同的修改来与执业者的意见保持一致。

## 提供书面声明

在审查过程中，实体管理层针对特定询问或通过提交描述和实体管理层的声明，向执业者做出许多口头和书面陈述。来自实体管理层的此类陈述是执业者获取的证据的一部分。然而，它们不能替代执业者可以合理预期可获得的其他证据，并且单凭它们本身也不足以就其所涉及的任何事项提供充分适当的证据。此外，执业者收到可靠书面陈述的事实并不影响执业者获取的其他证据的性质或范围。

确定与实体管理层或治理结构中的哪些适当人员或人士进行互动，由执业者负责，并考虑到哪些人员或人士对相关问题拥有适当的职责和知识。在某些情况下，执业者可能会从除实体管理层以外的各方获取书面陈述，例如那些负有治理责任的人员。

在某些情况下，做出声明的一方可能对所覆盖的声明中的特定事项负有间接责任并了解相关情况。例如，实体的首席信息官可能通过个人经验了解某些事项，通过向其汇报的员工了解其他事项。执业者可以要求直接 or 间接负责并了解书面陈述所覆盖事项的各方提供其自身的陈述。

书面陈述通常确认明确或隐含地提供给执业者的陈述，指示并记录此类陈述的持续适当性，并减少关于陈述主题事项发生误解的可能性。书面陈述通常采取责任方信函的形式，应做到以下几点：

- a. 基于相关准则，包含实体管理层关于每个主题事项⁶的声明。
- b. 声明：
  - i. 所有相关事项均反映在主题事项的计量或评估或声明中。
  - ii. 所有已知的与主题事项或声明相矛盾的事项，以及来自监管机构或影响主题事项或声明的其他任何沟通，均已披露给执业者，包括在书面声明所述期间结束之日与执业者报告日期之间收到的沟通。
- c. 承认对以下事项的责任：
  - i. 主题事项和声明，
  - ii. 选择准则，以及
  - iii. 确定此类准则是否适合实体管理层的用途。
- d. 声明实体管理层已向执业者提供了所有相关信息和访问权限。
- e. 声明实体管理层认为，未更正的错报（描述错报和控制缺陷）单独和总体上对主题事项均不重要。
- f. 声明实体管理层已在适当的情况下向执业者披露以下事项：
  - i. 实体或其员工在系统或生产、制造或分发的货物方面，违反法律法规、欺诈或未更正错报的事件（这些事件显然并非微不足道），以及此类事件是否已适当传达给受影响方。
  - ii. 供应商或业务伙伴在货物或系统方面，违反法律法规、欺诈或未更正错报的事件（这些事件显然并非微不足道），且实体知悉此类事件。
  - iii. 知悉任何可能对主题事项产生不利影响的实际、涉嫌或指控的故意行为。
  - iv. 任何已知控制设计方面的缺陷。
  - v. 所有控制措施未按描述运行的实例。
  - vi. 所有已识别的系统事件，这些事件导致在审查涵盖期间内实体实现其主要系统目标受到重大损害。
  - vii. 在系统描述涵盖期间之后、直至执业者报告日期为止，可能对描述、控制有效性或管理层声明产生重大影响的所有事件。

执业者可能需要其他陈述的事项通常取决于业务约定的事实和情况。例如，如果实体在分发货物之前检测到重大生产质量问题，执业者可能会决定就此事及其他类似事项请求陈述。

所需的书面陈述与实体管理层的书面声明是分开的，并作为补充。陈述的日期为执业者报告日期，并涉及执业者意见中提到的主题事项和期间。

如果实体使用供应商，且实体管理层选择使用包容性方法来描述所接收的原材料、组件或服务



from the supplier, the practitioner would also request many of the same representations listed in the previous paragraphs from management of the supplier.

### Requested Written Representations Not Provided or Not Reliable

The attestation standards provide guidance to the practitioner when:

- a. entity management has not provided one or more of the requested representations;
- b. the practitioner concludes that there is sufficient doubt about the competence, integrity, ethical values, or diligence of those providing the written representations; or
- c. the practitioner concludes that the written representations are otherwise not reliable.

In such circumstances, the guidance states that the practitioner should:

- a. discuss the matter with the appropriate party or parties,
- b. reevaluate the integrity of those from whom the representations were requested or received and evaluate the effect that this may have on the reliability of representations and evidence in general, and
- c. if any of the matters are not resolved to the practitioner's satisfaction, take appropriate action.

### Representations From the Engaging Party When Not the Responsible Party

When the engaging party is not the responsible party, the attestation standards require the practitioner to request written representations from the engaging party, in addition to those requested from the responsible party, in the form of a letter addressed to the practitioner. Those representations should do the following:

- a. Acknowledge that the responsible party is responsible for the subject matter and assertion.
- b. Acknowledge the engaging party's responsibility for selecting the criteria, when applicable.
- c. Acknowledge the engaging party's responsibility for determining that such criteria are appropriate for its purposes.
- d. State that the engaging party is not aware of any material misstatements in the subject matter or assertion.
- e. State that the engaging party has disclosed to the practitioner all known events subsequent to the period (or point in time) of the subject matter being reported on that would have a material effect on the subject matter or assertion.
- f. Address other matters as the practitioner deems appropriate.

### Modifying the Assertion

As previously discussed, entity management provides the practitioner with a written assertion about whether the description presents the system that was designed and implemented in accordance with the description criteria and over financial reporting when they are part of the user entity's information and communications component of internal control maintained by the service organization. Such an examination is known as a SOC 1ᵣ examination, and the resulting report is known as a SOC 1ᵣ report.

Service organizations frequently receive requests from user entities for these reports because they are needed by the auditors of the user entities' financial statements (user auditors) to obtain information about controls at the service organization that may affect assertions in the user entities' financial statements. A SOC 1ᵣ report is intended solely for the information and use of existing user entities (for example, existing customers of the service organization), their financial statement auditors, and management of the service organization. AICPA Guide Reporting on an Examination of Controls at a Service Organization Relevant to User Entities' Internal Control Over Financial Reporting (SOC 1ᵣ) contains application guidance for service auditors.

### SOC 2ᴿ — SOC for Service Organizations: Trust Services Criteria

An entity's management is responsible for assessing and addressing risks faced by the entity related to reporting, compliance with laws and regulations, and the efficiency and effectiveness of its operations. When an entity engages a service provider (referred to as a service organization in this context) to perform certain processes or functions, the entity (referred to as a user entity) exposes itself to additional risks related to the service organization's system. Although management of a user entity can delegate tasks or functions to a service organization, the ownership and responsibility for the product or service provided to customers of the user entity cannot be delegated. Management of the user entity is held responsible by those charged with governance (for example, board members), customers, shareholders, regulators, and other affected parties for establishing effective internal control over outsourced functions.

To assess and address the risks associated with an outsourced service, management of the user entity needs information about the service organization's controls over the system through which the services are provided. When assessing controls at a service organization that may be relevant to and affect the services provided to user entities, management of a user entity may ask the service organization for a service auditor's report on a description of the service organization's system and the design and operating effectiveness of controls over the service organization's system that may be relevant to the security, availability, or processing integrity of the system or the system's ability to maintain the confidentiality or privacy of the information processed for user entities. Obtaining a service auditor's report from a service organization provides management of the user entity with information that may be useful in assessing risk but does not relieve the user entity of its responsibilities with regard to an effective system of internal control.

In a SOC 2ᴿ engagement, the service auditor examines and reports on management's description of a service organization's system and the suitability of the design and operating effectiveness of the controls over its system relevant to security, availability, processing integrity, confidentiality, or privacy against the trust services criteria in TSP section 100. AICPA Guide SOC 2ᴿ Reporting on an Examination of Controls at a Service Organization Relevant to Security, Availability, Processing Integrity, Confidentiality, or Privacy provides guidance to service auditors engaged to perform a SOC 2ᴿ engagement.

### SOC 3® — SOC for Service Organizations: Trust Services Criteria for General Use Report

Similar to a SOC 2® engagement, in a SOC 3® examination the practitioner reports on whether controls within the system, which are necessary to provide reasonable assurance that the service organization achieved its service commitments and system requirements, were effective based on the applicable trust services criteria. Although the requirements and guidance for performing a SOC 3® examination are similar to a SOC 2® examination, the reporting requirements are different. Because of the different reporting requirements, a SOC 2® report is appropriate only for specified parties with sufficient knowledge and understanding of the service organization and the system, whereas a SOC 3® report is ordinarily appropriate for general use.

### SOC for Cybersecurity

Cybersecurity has become a top concern for boards of directors and senior executives of many entities, regardless of their size or the industry in which they operate. In addition, governmental officials are also concerned about cybersecurity at governmental agencies and departments. For most entities, cybersecurity is a significant business risk that needs to be identified, assessed, and managed along with other business risks the entity faces, and it is management’s responsibility to ensure that all employees throughout the entity, not only those in the information technology department, address cybersecurity risks. Managing this business issue is especially challenging because even an entity with a highly sophisticated cybersecurity risk management program has a residual risk that a material cybersecurity breach can occur and not be detected in a timely manner. Furthermore, the combined effects of an entity’s dependency on information technology, the complexity of information technology networks and business applications, extensive reliance on third parties, and human nature (for instance, susceptibility to social engineering) are only likely to increase the need for effective cybersecurity risk management programs in the foreseeable future.

For those reasons, entities have begun requesting practitioners to examine and report on a description of the entity’s cybersecurity risk management program and the effectiveness of controls within the program. This examination is known as a SOC for Cybersecurity examination; the related report is known as a cybersecurity risk management examination report. The performance and reporting requirements for such an examination are found in AT-C section 105, Concepts Common to All Attestation Engagements, and AT-C section 205, Examination Engagements. AICPA Guide Reporting on an Entity’s Cybersecurity Risk Management Program and Controls contains interpretive application guidance for practitioners performing these engagements.

The cybersecurity risk management examination report includes three key components: (1) the description of the entity’s cybersecurity risk management program, (2) management’s assertion about whether the description is presented in accordance with the description criteria and whether the controls within the cybersecurity risk management program were effective to achieve the entity’s cybersecurity objectives based on the control criteria, and (3) the practitioner’s opinion about whether the description is presented in accordance with the description criteria and whether the controls within the cybersecurity risk management program were effective to achieve the entity’s cybersecurity objectives based on the control criteria.





## 实体管理信息

在网络安全风险管理审查中，管理层选择用于编制实体网络安全风险管理计划描述的准则（描述准则）以及用于评估该计划内控制有效性的准则（控制准则）。AICPA 指南 *Reporting on an Entity's Cybersecurity Risk Management Program and Controls* 包含描述准则以及安全、可用性和保密性的受托服务准则，可在网络安全风险管理审查中使用。

由于实务人员的报告旨在纳入网络安全风险管理审查报告中，而该报告 intended for general distribution，因此实务人员的报告适用于一般用途。尽管如此，实务人员可能会决定将报告的使用限制于特定用户。

![](img/149dd1483d2367db7b44fe917db957ec_274_0.png)
