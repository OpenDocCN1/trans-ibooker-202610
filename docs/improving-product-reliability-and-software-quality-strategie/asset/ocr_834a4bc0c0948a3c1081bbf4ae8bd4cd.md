defined, assign reliability budgets for all the subsystems, circuit boards, and mechanical assemblies that make up the system or product. The sum of all the subsystem reliability budgets must be equal to the system reliability defined earlier.

If the reliability is defined in MTBF (in hours), then add up the reciprocal of each MTBF. The MTBF budget numbers for each of the subsystems cannot be added directly. If the reliability for the subsystems is defined in FITs (failures in time, per billion hours), then they can be directly added to determine the equivalent system FIT rate. FIT numbers can be converted to MTBF by taking the reciprocal of the FIT number and multiplying it by one billion to arrive at MTBF numbers in hours.

```
MTBF = (1 / FIT) × (1 × 10⁹)
```

An example of how the subsystem MTBF budgets can be added together to equal the system reliability budget in shown Figure 19.4. Suppose that you are working on a new product development whose system reliability budget (stated in MTBF) was defined in the product concept phase to be 10,000 hours. In the design concept phase, the system architecture shows the system to consist of five major subsystems. The reliability organization is then tasked to define the MTBF budgets for the five major subsystems. The MTBF budgets represent reliability's best guess at what the MTBF requirements need to be for the different subsystems. If the widget is a completely new product, then the reliability budgets may be nothing more than an educated guess for each of the different subsystems. However, if the new product has many similarities to previous products, there is a database of knowledge regarding the previous product's reliability, broken down into their subsystems to the board level. A failure reporting, analysis, and corrective action system (FRACAS) will contain information regarding the previous product's reliability, which can be broken down into its system and subsystem components. To learn more about MTBF refer to Chapter 6 and Appendix B.

System MTBF

```
= 1 / ( (1 / MTBF₁) + (1 / MTBF₂) + (1 / MTBF₃) + (1 / MTBF₄) + (1 / MTBF₅) )
```

```
= 1 / ( (1 / 17,000) + (1 / 28,000) + (1 / 350,000) + (1 / 940,000) + (1 / 650,000) )
```

```
= 10,000
```

How does one determine the budget numbers? The best way is to review the past performance of similar subassemblies, the key contributors to this performance, and the expected result if these issues were removed in the new product.

The first time you set the reliability budget (for each subsystem) will always be the hardest. With each new program it will get easier. This is because you can leverage the lessons learned from the previous programs and have a better idea as to what the reliability budgets should be for the subsystem and circuit board levels. Appendix B discusses MTBF calculations in more rigorous detail.