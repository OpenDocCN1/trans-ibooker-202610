
## 来自ARULEAV的EMA 51：RUL与PH估计

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_335_0.png)

## 来自ARULEAV的SoH估计

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_335_1.png)

图6.85 EMA（负载）曲线图：使用基于DPS的FFS和ARULEAV得到的RUL、PH和SoH估计值。

一次一个数据点（时间和幅值），从而得到数据和性能指标：图6.85和表6.3。

从记录的输出文件中，你可以收集并计算以下信息（以小时为单位）：估计BD发生在441小时——与实际开始退化时的444小时相差3小时；BDDET为504小时；使用估计的BD计算得出BD_OFF为63小时；EOL在3168小时检测到——恰好发生在功能失效之前；估计EOL为3164小时；PD_MAX计算为2727（3148减441）；PD_FOM计算为97.7%；PH_25在576小时达到；PH_10在1368小时达到。根据这些信息，你可以计算以下指标：χ_25为97.3%（根据((2727 – (576 – 441))/2727)），χ_10为68.3%，这些结果非常优异，超过我们推荐的70%和35%的目标。结果汇总见表6.4。

这些曲线图和性能指标证实，无需进行FFP到DPS的转换：FFP签名仅呈略微曲线状。事实上，在绘制（图6.85）并进行其他分析后，你和客户对RUL、PH和SOH感到欣喜若狂。

## CBD-绕组故障始于1000

## FFP - 绕组故障始于1000

## EMA FFS，节点61：绕组故障始于1000

图6.86 EMA节点61（绕组）的CBD、FFP和FFS。

## 来自ARULEAV的EMA节点61的RUL与PH估计

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_338_0.png)

## 来自ARULEAV的EMA节点61的SoH估计

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_338_1.png)

图6.87 EMA（绕组）曲线图：RUL、PH和SoH估计值；基于DPS的FFS和ARULEAV。

状态；并完成了对节点51（退化，载荷/摩擦类故障）、节点52（未退化）、节点61（退化，电机绕组类故障）和节点62（退化，功率开关晶体管故障）的测试。

接下来，你使用原型PHM系统对所有六个节点进行并发测试：这是一项集成功能测试。系统产生的信息如图6.91（启动初始化）、图6.92（节点警报，第1部分）和图6.93（节点警报，第2部分）所示。测试结束后，你绘制并检查输出文件，验证结果是否符合预期——它们没有改变。

### 6.12.2 系统性能指标：汇总

表6.4列出了在系统验证期间执行的功能测试的部分性能测量值和指标。

## CBD-晶体管故障始于700

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_339_0.png)

## 平滑FFP-C相晶体管故障始于700

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_339_1.png)

## 平滑FFS-3相晶体管故障始于700

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_339_2.png)

图6.88 EMA节点62（功率晶体管）的CBD、FFP和FFS。

```
%************************************************************************
%** PHM CONTROL & DATA FLOW
****************************************************************************
    Housekeeping and initialization
        Read SND
        For each defined node (NDEF) in SND
            Read, parse, and evaluate the NDEF
            Initialize operating parameters, variables, and constants
            Initialize work areas for checkpoint/restart
    Main
        When Node active
            Restore (or initialize) operating environment
                Call DDPID to get data (output DD bus)
                Call FV PID to process data (output FV bus)
                Call PIPID to produce prognostic information (output PI bus)
            Save operating environment
            When node functionally fails or is made inactive
                Call PAPID (if specified) to analyze performance
            Point to next node (in the ring)
        Otherwise
            Point to next node
end
```

图6.90 PHM：高层控制与数据流。

```
>> DXPHM01 ('-1','DXSNDIVP1','-1')
I4: AN INITIALIZED SND (DEFINITIONS) FILE HAS BEEN FOUND
I19: RECEIVED RESPONSE: REPLACE
I1: SAVED NODE DEFINITION FILE IS REPLACED: C:\DXPHMLIB\DXCPT\ND_50.mat
I1: SAVED NODE DEFINITION FILE IS REPLACED: C:\DXPHMLIB\DXCPT\ND_51.mat
I1: SAVED NODE DEFINITION FILE IS REPLACED: C:\DXPHMLIB\DXCPT\ND_52.mat
I1: SAVED NODE DEFINITION FILE IS REPLACED: C:\DXPHMLIB\DXCPT\ND_60.mat
I1: SAVED NODE DEFINITION FILE IS REPLACED: C:\DXPHMLIB\DXCPT\ND_61.mat
I1: SAVED NODE DEFINITION FILE IS REPLACED: C:\DXPHMLIB\DXCPT\ND_62.mat

I18: INITIALIZED NODE 50: OPENING FILE C:\DXPHMLIB\DXINDATA\DXDATFR1.txt
I18: INITIALIZED NODE 51: OPENING FILE C:\DXPHMLIB\DXINDATA\DXDAT_EMA10.txt
I18: INITIALIZED NODE 52: OPENING FILE C:\DXPHMLIB\DXINDATA\DXDAT_EMA20.txt
I18: INITIALIZED NODE 60: OPENING FILE C:\DXPHMLIB\DXINDATA\DXDATFR2.txt
I18: INITIALIZED NODE 61: OPENING FILE C:\DXPHMLIB\DXINDATA\DXDAT_EMA30.txt
I18: INITIALIZED NODE 62: OPENING FILE C:\DXPHMLIB\DXINDATA\DXDAT_EMA40.txt
```

图6.91 初始化：系统节点。

- 升级当前的预测分析程序，使其包含表6.4所示的所有测量、估计和计算，其中包含一些手工计算值。
- 可视化服务支持：使用GUI而不是你之前使用的EUI。示例如图6.94所示。

下一步是准备一套更新的需求、目标和规范。你可能会发现，下一阶段需要与客户紧密合作，这很可能耗时且有时令人沮丧：

- 故障管理。你需要理解并考虑客户当前以及未来可能的维护和供应

## 图6.92 系统警报，第1部分。

```
W19: SOH for NODE: 51 IS 99.8663 PERCENT
I44: RUL IS 4776.1 [hr] AT DATA TIME: 504 [hr]
I43: RECEIVED RESPONSE: CONTINUE
W19: SOH for NODE: 62 IS 99.9713 PERCENT
I44: RUL IS 4795.8 [hr] AT DATA TIME: 960 [hr]
I43: RECEIVED RESPONSE: CONTINUE
W19: SOH for NODE: 61 IS 99.8078 PERCENT
I44: RUL IS 4695.3 [hr] AT DATA TIME: 1104 [hr]
I43: RECEIVED RESPONSE: CONTINUE
W19: SOH for NODE: 50 IS 99.9087 PERCENT
I44: RUL IS 4786.1 [hr] AT DATA TIME: 1368 [hr]
I43: RECEIVED RESPONSE: CONTINUE
W19: SOH for NODE: 61 IS 74.4394 PERCENT
I44: RUL IS 1312.3 [hr] AT DATA TIME: 1536 [hr]
I43: RECEIVED RESPONSE: CONTINUE
W19: SOH for NODE: 51 IS 74.4327 PERCENT
I44: RUL IS 2114.7 [hr] AT DATA TIME: 1224 [hr]
I43: RECEIVED RESPONSE: CONTINUE
```

系统。例如，客户可能希望以下所有内容都作为警报响应得到支持：

- 继续操作。
- 暂停操作。
- 安排维护、拆卸更换和检查。
- 监控：更新/更改采样率和/或警报级别。
- 离线：结束操作。
- 在线：恢复已暂停或离线的预测目标的运行。
- 节点：为系统添加、更新或删除节点定义（NDEF）。

可变采样。你可能会发现，每组LRU根据其使用方式和位置而有不同的采样要求。例如，用于调整驾驶舱座椅的EMA与用于定位机翼表面的EMA相比，其关键程度可能较低。

预测分析程序。即使你可能认为该程序的设计主要在你的专业领域内，你也可能会发现客户另有想法。例如，程序是否应分析5%、10%和25%的alpha精度的细节？你的客户可能希望你的程序执行以下任何一项或全部操作：

- 按照PGALPHA的规定，分析并显示预测目标的精度统计信息（参见图6.62）。
- 发出警报时，分析并显示估计RUL值的估计精度。
- 仅在明确要求时绘制测量值和指标。

可视化服务。为PHM系统设计GUI可能是系统中最困难、最耗时且成本最高的方面，特别是由于以下原因：

- 你偏好的外观和感觉可能不被客户喜欢。
- GUI必须与现有的诊断和/或预测解决方案兼容并集成。
- 功能的广度和深度需要涵盖设计系统的所有方面：定义、程序、操作规范等。此外，每次添加、删除或更新功能都可能需要设计、审查、测试和批准的成本和时间。
