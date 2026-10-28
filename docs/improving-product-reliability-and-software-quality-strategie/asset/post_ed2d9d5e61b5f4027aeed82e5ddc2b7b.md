
## 19.7 Reflect on What Worked Well

The last activity before the concept phase ends and the design phase begins is reflection. In the reflection step you look back at the concept phase to see what worked and what didn’t. Capture the lessons learned early so that they are not forgotten and destined to be repeated. Document the findings and recommendations so that they may be incorporated into future programs. Reflection allows for continuous improvement of the reliability process.

## 20
## Product Design Phase

### 20.1 Product Design Phase

Now that the concept, requirements, and architecture for the product have been completely defined, the design of the product can commence. In the product design phase, everything required to produce a working prototype is developed. At the end of this phase, there will be a complete product documentation package. This will include the schematic, theory of operation, outline drawing, bill of materials (BOM), software, assembly, and mechanical drawings. There will also be a working prototype suitable for design validation, which will be performed in the next phase of product development. The decisions made by the end of this phase will determine the product cost, design, manufacturing, test, and service complexity and will also determine how difficult it will be to ramp production. Unless the product is completely redesigned, 80–90% of the product cost is determined. Cost down efforts to reduce product cost here are usually limited to reducing material cost because redesign at this point is not cost-effective.

Product cost is emphasized because there is a cost associated with reliability. The cost equation works two ways. First, there is an added material cost associated with using higher reliability components and adding redundancy. The added cost should be evaluated against the improvement expected in reliability and how much the market is willing to pay for increased reliability. There is also a cost saving associated with designing the product right the first time. Typically, early in the product launch, there is an extensive number of engineering design changes to the product (refer to Chapter 1). Design changes at the end of the product development increase the cost to produce the product and can reduce product reliability when there is extensive rework and retrofit required.

The design phase has two parts (Figure 20.1), a product design phase and a design validation phase. The majority of the engineering design activity takes place in the design phase. The product development team has been relatively small up to this point and increases significantly to design the product, produce a working prototype, and create a documentation package. A well-designed and reliable product that is manufacturable, brings the added benefit of ensuring that the design team is released to work on the next project. If there are problems with the design, then engineering support will be needed to fix design problems. Pulling back key design resources to fix past problems negatively impacts resources needed to support future product designs. Some companies get around this by forming a sustaining engineering group. Their focus is to resolve design problems after a product has been released to manufacturing. The sustaining engineering group typically consists of less-experienced designers and engineers to fix

![](img/50dc187750ab899846d825394ccbb93a_302_0.png)

Figure 20.1 The first phase of the product life cycle.

designs in which they were not involved. When there is a sustaining group to fix problems, it is vital that their findings make it back to the design team so that improvements are put in place to prevent the problem being repeated.

There is usually a cost associated with developing a more reliable design. Warranty cost savings will often justify these early-added costs. The capital expense, engineering resources, and time required to fix design problems can be significant. When sustaining engineering activity costs are added, this fuller picture will provide a better indication of what the actual cost is of an unreliable product.

Finally, the design phase is the last opportunity where the product design can be proactively improved. Once a product enters the design validation phase, the costs and schedule impact of design changes increase significantly. The problems found in product validation are more expensive to fix and can impact the product release date. Therefore, significant focus needs to be given in the product design phase to ensure that the product design is reliable.

There are eight reliability activities that take place in the product design phase (Table 20.1).

### 20.2 Reliability Estimates

As the program proceeds from concept to design, the materials required to produce the product are defined. Once the BOM is known, a reliability estimate can be performed. The reliability estimates are performed for each circuit board and subsystem that makes up the system. The estimate considers each item in the BOM and matches it to a reliability number. This reliability number is often expressed in FITs (failures in time per billion hours). Each BOM item's FIT number is added together to calculate the total FIT. The sum of all the individual FITs provides an estimate of the reliability for that board or subsystem. Reliability engineering is usually responsible for determining the reliability estimates for system, subsystems, and circuit boards. Estimating the reliability is more complicated when redundancy is involved or the components do not have a constant failure rate. The mathematics to deal with this is complex and beyond the scope of the book. Those who wish to gain a better understanding of this subject are referred to the bibliography section at the end of this chapter.

Reliability estimates provide insight into the areas where the product is likely to be least reliable. These estimates also point to those BOM items that are the major contributors to lowering the reliability estimate. There will not be sufficient resources (capital and personnel) to perform all the reliability tests on everything or to life test every

# Table 20.1 Reliability activities for the product design phase.

| Participants | Product design phase | Product design phase |
|---|---|---|
| Participants | Reliability activities | Deliverables |
| • Reliability | 1. Reliability estimates developed for all lower level assemblies. Identify all items, early wear out items. | 1. Reliability estimate spreadsheet and a Pareto by component and subassembly highest failure rate items. Pareto early wearout items for service and maintenance strategies. |
| • Marketing<br>• Design engineering (electrical, mechanical, software, thermal, etc.)<br>• Manufacturing engineer<br>• Test engineer<br>• Field service/customer support<br>• Purchasing/supply management<br>• Safety & regulation | 2. Implement risk mitigation plans.<br>3. Apply reliability design guidelines (DFM, DFR, DFT, DFS).<br>4. Perform design FMEA.<br>5. Install FRACAS.<br>6. Begin HALT planning.<br>7. Update lessons learned.<br>8. Review status and agree on each risk issue and mitigation plans. Hold risk mitigation meeting. | 2. Risk mitigation meeting and agreement to proceed to next phase.<br>3. Checklist or review that design guidelines has been followed and variances are acceptable.<br>4. Completed FMEA spreadsheet (Table 7.1) and closure on FMEA action items.<br>5. Structured FRACAS database and user input interface.<br>6. Detailed plan and schedule for HALT.<br>7. Updated lessons learned database, communicate to design team new issues and revise risk mitigation plan if needed.<br>8. Risk mitigation meeting and agreement to proceed to next phase. |

Note: DFM: design for manufacturing; DFR: design for reliability; DFT: design for test; DFS: design for service (and maintainability); FMEA: failure modes and effects analysis; FRACAS: failure reporting, analysis, and corrective action system; HALT: Highly Accelerated Life Test.

component. By knowing where the reliability problems are likely to reside, the vital resources can be focused on the areas where the product is expected to be the least reliable. The reliability estimates are one technique to ensure that there is significant reliability focus around those areas of the product that are expected to be the least reliable.

The reliability estimate, when sorted from the highest (negative) impact on reliability down to the lowest (least) impact on reliability, provides a Pareto list of components to focus reliability improve effort. The Pareto identifies the components that are most significantly impacting reliability. In these situations, you can evaluate alternative components or design changes (including strategies like redundancy) to evaluate the reliability improvement. Reliability estimates may also help in determining the need to spend more for a more reliable component.
