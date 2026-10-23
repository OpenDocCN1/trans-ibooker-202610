
be eliminated from the process, variability will be reduced and the process will be improved; (ii) the control chart will only detect assignable causes. Management, operator, and engineering action will usually be necessary to eliminate the assignable cause. A long-term plan for responding to control chart signals is vital (Montgomery et al. 2011).

In identifying and eliminating assignable causes, it is important to find the underlying root cause of the problem and to attack it. A cosmetic solution will not result in any real, long-term process improvement. Developing an effective system for corrective action is an essential component of an effective SPC implementation. The control chart may also be used as an estimating device. That is, from a control chart that exhibits statistical control, we may estimate certain parameters, such as the mean, standard deviation, and fraction nonconforming or fallout. These estimates may then be used to determine the capability of the process to produce acceptable products. Such process capability ($PC$) studies have considerable impact on many management decision problems that occur over the product cycle, including make-or-buy decisions, plant and process improvements that reduce process variability, and contractual agreement with customers or suppliers regarding product quality.

Control charts for quality characteristics are called attributes control charts. Control charts have had a long history of use in industry. There are at least five reasons for their popularity:

- 1) Control charts are a proven technique for improving productivity. A successful control chart program will reduce scrap and rework, which are the primary productivity killers in any operation. If you reduce scrap and rework, productivity increases, cost decreases, and production capacity (measured in the number of good parts per hour) increases.

- 2) Control charts are effective in defect prevention. The control chart helps keep the process in control, which is consistent with the "do it right the first time" philosophy. It is never cheaper to sort out the "good" units from the "bad" later on than it is to build it right initially. If you do not have effective process control, you are paying someone to make a nonconforming product.

- 3) Control charts prevent unnecessary process adjustments. A control chart can distinguish between background noise and abnormal variation; no other device, including a human operator, is as effective in making this distinction. If process operators adjust the process based on periodic tests unrelated to a control chart program, they will often overreact to the background noise and make unneeded adjustments. These unnecessary adjustments can usually result in a deterioration of process performance.

- 4) Control charts provide diagnostic information. Frequently, the pattern of points on the control chart will contain information that is of diagnostic value to an experienced operator or engineer. This information allows the operator to implement a change in the process that will improve the performance.

- 5) Control charts provide information about PC. The control chart provides information about the value of important process parameters and their stability over time, which allows an estimate of PC to be made. This information is of tremendous use to product and process designers.

Control charts are among the most effective management control tools, and they are as important as cost controls and material controls. Modern computer technology has made it easy to implement control charts in any type of process because data collection and analysis can be performed on a microcomputer or a local area network terminal in real time, online at work center (Montgomery et al. 2011).

## 8.4 Process Capability

In any manufacturing process and operation, variability exists in the process output. In a machining operation, which is one of the most accurate processes, the machined parts may appear to be identical, but close inspection reveals dimensional differences from one part to the next. Manufacturing variations can be divided into two types: random and assignable.

Random variations are caused by many factors: human variability with each operation cycle, variations in raw material, machine vibration, and so on. Individually, these factors may not amount to much, but collectively the errors can be significant enough to cause trouble unless they are within the tolerances for the part. Random variations typically form a normal statistical distribution. The output of the process tends to cluster about the mean value, in terms of the product's quality characteristic of interest (e.g., length, or diameter, or weight). A large proportion of the population of parts is centered around the mean, with fewer parts away from the mean. When the only variations in the process are of this type, the process is said to be in statistical control. This kind of variability will continue as long as the process is operating normally. It is when the process deviates from this normal operating condition that variations of the second type appear.

Assignable variations indicate an exception from normal operating conditions. Something has occurred in the process that is not accounted for by random variations. Reasons for assignable variations include operator mistakes, defective raw materials, tool failures, machine malfunctions, and so on. Assignable variations in manufacturing usually betray themselves by causing the output to deviate from the normal distribution. The process is no longer in statistical control.

PC relates to the normal variations inherent in the output when the process is in statistical control. By definition, PC equals ±3 standard deviations about the mean output value (a total of six standard deviations):

```
PC = μ ± kσ
```

where PC, process capability; μ, process mean, which is set at the nominal value of the product characteristic; k is the “distance” of control limits from the center line, expressed in standard deviation units. A common choice is k = 3 and σ, standard deviation of the process. Assumptions underlying this definition are (i) the output is normally distributed and (ii) steady-state operation has been achieved and the process is in statistical control. Under these assumptions, 99.73% of the parts or products produced will have output values that fall with ±3.0 of the mean. This general theory of control charts was first proposed by Dr. Walter A. Shewhart, and control charts developed according to these principles are often called Shewhart control charts.

### 8.4.1 Statistical Process Control

SPC involves the use of various statistical methods to assess and analyze variations in a process. SPC methods include simply keeping records of the production data, histograms, PC, and control charts. Control charts are the most widely used SPC method, and our discussion in this section will focus on them.

The underlying principle in control charts is that the variations in any process divide into two types (Section 8.4): (i) random variations, which are the variations present if the process is in statistical control and (ii) assignable variations, which indicate a departure from statistical control. It is the objective of a control chart to identify when the process has gone out of statistical control, thus signaling that some corrective action should be taken.

A control chart is a graphical technique in which statistics computed from measured values of a certain process characteristic are plotted over time to determine if the process remains in statistical control. The general form of the control chart is illustrated in Figure 8.6. The chart consists of three horizontal lines that remain constant over time: a center, a lower control limit (LCL), and an upper control limit (UCL). The center is usually set at the normal design value. The UCL and LCL are generally set at ±3 standard deviation of the sample means.

![](img/009e9fca58d02c8d25e9fb0817ce46c9_390_0.png)

Figure 8.6 A control chart set at ±3 standard deviations of the sample means.

It is highly unlikely that a sample drawn from the process lies outside the UCL or LCL while the process is in statistical control. Thus, if it happens that a sample value does fall outside these limits, it is interpreted to mean that the process is out of control. Therefore, an investigation is undertaken to determine the reason for the out-of-control condition, with appropriate corrective action to eliminate the condition. By similar reasoning, if the process is found to be in statistical control and there is no evidence of undesirable trends in the data, then no adjustments should be made, since they would introduce an assignable variation to the process. The philosophy “if it ain’t broken, don’t fix it” is applicable in control charts.

There are two basic types of control charts: (i) control charts for variables and (ii) control charts for attributes. Control charts for variables require a measurement of the quality characteristic of interest. Control charts for attributes simply require a determination of whether a part is defective or how many defects there are in the sample.

### 8.4.2 Control Charts for Variables

A process that is out of statistical control manifests this condition in the form of significant changes in (i) process mean and/or (ii) process variability. Corresponding to these possibilities, there are two principal types of control charts for variables: (i) x̄-chart and (ii) R-chart. The x̄-chart (call it “x-bar chart”) is used to plot the average measured value of a certain quality characteristics for each of a series of samples taken from the production process. It indicates how the process mean changes over time. The R-chart plots the range of each sample, thus monitoring the variability of the process and indicating whether it changes over time.
