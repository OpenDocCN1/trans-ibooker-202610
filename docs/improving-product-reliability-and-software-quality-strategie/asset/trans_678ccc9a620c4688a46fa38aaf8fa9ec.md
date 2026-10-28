

## 进一步阅读
### FMEA
S. Bednarz, D. Marriot, Efficient Analysis for FMEA, 1998 Proceedings Annual Reliability and Maintainability Symposium (1998).

M. Kennedy, Failure Modes and Effects Analysis (FMEA) of Flip-Chip Devices Attached to Printed Wiring Boards (PWB), IEEE/CPMT International Manufacturing Technology Symposium, IEEE (1998).

M. Krasich, Use of Fault Tree Analysis for Evaluation of System Reliability Improvements in Design Phase, 2000 Proceedings Annual Reliability and Maintainability Symposium (2000).

K. Onodera, Effective Techniques of FMEA at Each Life-Cycle Stage, 1997 Proceedings Annual Reliability and Maintainability Symposium, IEEE (2000).

Prasad, S. (1991). Improving manufacturing reliability in IC package assembly using the FMEA technique. IEEE Transactions of Components, Hybrids and Manufacturing Technology 14 (3): 452–456.

D. J. Russomanno, R. D. Bonnell, J. B. Bowles, Functional Reasoning in a Failure Modes and Effects Analysis (FMEA) Expert System, 1993 Proceedings Annual Reliability and Maintainability Symposium, IEEE (1993).

SAE Recommended Failure Modes and Effects Analysis (FMEA) Practices for Non-Automobile Applications, SAE (Reaffirmed 2012) https://www.sae.org/standards/content/arp5580/.

R. Whitcomb, M. Riox, Failure Modes and Effects Analysis (FMEA) System Development in a Semiconductor Manufacturing Environment, IEEE/SEMI Advanced Semiconductor Manufacturing Conference, IEEE (1994).

### 加速方法
H. Caruso, A. Dasgupta, A Fundamental Overview of Accelerated-Testing Analytic Models, 1998 Proceedings Annual Reliability and Maintainability Symposium, IEEE (1998).

M. J. Cushing, Another Perspective on the Temperature Dependence of Microelectronic-Device Reliability, 1993 Proceedings Annual Reliability and Maintainability Symposium (1993).

J. Evans, M. J. Cushing, P. Lall, R. Bauernschub, A Physics-of-Failure (POF) Approach to Addressing Device Reliability in Accelerated Testing of MCMS, IEEE (1994).

Lall, P. (1996). Tutorial: temperature as an input to microelectronics-reliability models. IEEE Transactions on Reliability 45 (1): 3–9.

### ESS
H. Caruso, An Overview of Environmental Reliability Testing, 1996 Proceedings Annual Reliability and Maintainability Symposium, IEEE (1996).

### HALT
H. Caruso, A. Dasgupta, *A Fundamental Overview of Accelerated-Testing Analytic Models*, 1998 Proceedings Annual Reliability and Maintainability Symposium, pp. 389–393 IEEE (1998).
M. R. Cooper, *Statistical Methods for Stress Screen Development*, 1996 Electronic Components and Technology Conference, IEEE (1996).
G. A. Epstein, *Tailoring ESS Strategies for Effectiveness and Efficiency*, 1998 Proceedings Annual Reliability and Maintainability Symposium, 37–42, IEEE (1998).
S. M. Nassar, R. Barnett, *Applications and Results of Reliability and Quality Programs*, 2000 Proceedings Annual Reliability and Maintainability Symposium, IEEE (2000).

J. A Anderson, M. N. Polkinghome, *Application of HALT and HASS Techniques in an Advanced Factory Environment*, 5th International Conference on Factory 2000 (April, 1997).
C. Ascarunz, *HALT: Bridging the Gap Between Theory and Practice*, International test Conference 1994, IEEE (1994).
R. Confer, J. Canner, T. Trostle, S. Kurz, *Use of Highly Accelerated Life Test Halt to Determine Reliability of Multilayer Ceramic Capacitors*, IEEE (1991).
N. Doertenbach, *High Accelerated Life Testing – Testing With a Different Purpose*, IEST, 2000 proceedings (February, 2000).
General Motors Worldwide Engineering Standards, *Highly Accelerated Life Testing*, GM (2002).
R. H. Gusciaoa, *The Use of Halt to Improve Computer Reliability for Point of Sale Equipment*, 1998 Proceedings Annual Reliability and Maintainability Symposium, IEEE (1998).
HnateK, E.R. (1999). *Let HALT Improve Your Product*. Nelson Publishing.
Hobbs, G.K. (1997). What HALT and HASS can do for your products. In: *Hobbs Engineering, Evaluation Engineering*, 138. Qualmark Corporation.
Hobbs, G.K. (2000). *Accelerated Reliability Engineering*. Wiley.
Hobbs, G.K. (1977). *What HALT and HASS Can Do for Your Products*. Nelson Publishing Inc.
Joseph Capitano, P.E. (1998). Explaining accelerated aging., *Evaluation Engineering* 46.
McLean, H. (1991). *Highly Accelerated Stressing of Products with Very Low Failure Rates*. Hewlett Packard Co.
McLean, H.W. (2000). *HALT, HASS & HASA Explained: Accelerated Reliability Techniques*. American Society for Quality.
Minor, E.O. *Quality Maturity Earlier for the Boeing 777 Avionics*. The Boeing Company.
M. L. Morelli, *Effectiveness of HALT and HASS*, Hobbs Engineering Symposium, Otis Elevator Company (1996).
D. Rahe, *The HASS Development Process*, ITC International Test Conference, IEEE (1999).
D. Rahe, *The HASS Development Process*, 2000 Proceedings Annual Reliability and Maintainability Symposium, IEEE (2000).
M. Silverman, *Summary of HALT and HASS Results at an Accelerated Reliability Test Center*, Qualmark Corporation, Santa Clara, CA, 1998 Proceedings Annual Reliability and Maintainability Symposium, IEEE (1998).

M. Silverman, *HASS Development Method: Screen Development, Change Schedule, and Re-Prove Schedule*, 1998 Proceedings Annual Reliability and Maintainability Symposium, IEEE (1998).
Silverman, M.A. (n.d.). *HALT and HASS on the Voicememo II™*. Qualmark Corporation.
Silverman, M. (1998). *Summary of HALT and HASS Results at an Accelerated Reliability Test Center*. Santa Clara, CA: Qualmark Corporation.
Silverman, M. (n.d.). *Why HALT Cannot Produce a Meaningful MTBF Number and Why this Should Not be a Concern*. Santa Clara, CA: Qualmark Corporation, ARTC Division.
J. Strock, Product Testing in the Fast Lane, *Evaluation Engineering* March, (2000).
Tustin, W. and Gray, K. Don’t let the cost of HALT stop you. *Evaluation Engineering* 36–44.

## 22
## 软件测试与调试

显然，在发布软件之前需要对其进行测试。测试有多个阶段，按顺序执行以实现最有效的缺陷检测。首先由开发人员测试其代码，然后交由软件质量保证（SQA）继续测试。开发人员测试包括单元测试和集成测试。SQA 测试侧重于系统测试。单元测试在隔离状态下对单个组件进行测试。集成测试以受控方式检查模块之间的交互。系统测试对整个软件包进行测试。测试应按以下顺序进行：

Unit testing → Integration testing → System testing

每个步骤成功完成后再开始后续的测试步骤。按此顺序测试比尝试以不同顺序运行这些测试要高效得多。当先测试单个组件（在单元测试期间）时，后续测试可以专注于查找集成或系统缺陷，同时确信各个模块相对稳健。如果一次性测试所有软件，就很难找到缺陷的根本原因。花费大量时间试图确定缺陷所在的位置。这会导致生产率显著下降。有关不同类型测试的详细信息可在以下各节中找到。

### 22.1 单元测试

单元测试针对最小的可测试代码单元运行，即方法、函数或过程（取决于语言）。单元测试的目的是在将软件交付或在更高级别测试之前，确保最低层的代码正常工作。单元测试由软件开发人员运行。它们应在代码交付到代码仓库之前运行。在将软件移交给 SQA 组织进行测试之前也一定应运行它们。

通常，软件开发人员会编写特定的单元测试代码来测试其低层代码。通常，单元测试和产品软件开发是迭代过程。软件开发人员会编写产品代码，然后执行其单元测试，发现错误，修复代码，并重复直到所有单元测试通过。在许多情况下，单元测试是在编写产品代码之前编写的。有一个开发方法称为测试驱动设计，其中所有测试都在任何代码开发之前编写，作为一种手段

