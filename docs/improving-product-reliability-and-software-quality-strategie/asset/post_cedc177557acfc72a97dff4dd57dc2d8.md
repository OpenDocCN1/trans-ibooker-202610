
The design guidelines are defined in black and white, but there will be gray areas in their interpretation. Unfortunately, not every application is black and white. These shades of gray represent potential risk areas and sometimes require testing or outside expert opinion for resolution. As the gray areas are resolved, the DFR guidelines are revised to reflect changes. The guidelines will probably not cover every aspect of the design, especially those associated with leading-edge technologies. These issues are better captured in a risk mitigation plan with the results incorporated into the design guidelines. The guidelines are to be continually revised to reflect continuous improvements, and reflect present technology and new reliability issues that surface in the existing product.

Applying reliability guidelines involves evaluating the trade-offs that affect product failure rate, repair cost, safety, image, profit, and time to market. The design decisions for increased reliability can have a negative impact on the product cost, ease of manufacturing, and design complexity. It is best to avoid over design or designing a product that no one is willing to pay for. This requires a marketing understanding of the reliability requirements, design trade-offs, liabilities, and warranty cost and consumer impact. The impact from selling an unreliable product may not be known for years. However, expect a trend of reduced market share as word spreads out about customer dissatisfaction. The cost of an unreliable product also manifests itself in rework, scrap material, and expensive recalls identified late in the product life cycle.

There is an additional cost factor associated with lost business from a dissatisfied customer. Today’s consumer is much more knowledgeable about product reliability. With the advent of the internet, dissatisfied customers have a much greater impact on future sales through chat rooms and customer product reviews that are now found at popular sites. This may be the greatest threat. Today’s consumer is computer smart and can easily research a product’s reliability history. Unfortunately, most of the product reviews on these web pages are the result of dissatisfied customers. To survive, the products developed and produced must be reliable. The best way to achieve this and to meet the demanding time-to-market requirements is by incorporating DFR guidelines in a concurrent engineering effort.

Most companies today have design for manufacturing (DFM) and design for test (DFT) guidelines as part of the product development process. The benefits from DFM and DFT are well understood. Design teams understand how DFM and DFT reduce product development time and reduce the number of engineering changes at product release. The DFM and DFT guidelines are applied in the design phase and usually incorporate a checklist to verify that past mistakes are not repeated. These DFM and DFT guidelines have been developed from lessons learned over time. These lessons are then communicated through a set of guidelines and become part of the checklist required for final design approval and sign-off.

The same techniques and processes that are used in DFM and DFT apply to reliability design guidelines. The process for applying design guidelines is already in place; the problem for most companies is that they do not have reliability design guidelines and do not know how to create them. When it comes to reliability design guidelines, it seems we are back in the Stone Age where the design is thrown over the fence to manufacturing. Then it becomes the responsibility of the quality and manufacturing team or sustaining engineers groups.

| Requirements/options: | Reliability impact/benefit: |
|---|---|
| 1. Temperature stress (Rule #1):<br>For every 10 °C increase in temperature the useful life decreases by a factor of 2.<br><br>2. Ripple current stress (Rule #2):<br>Stay below 50% of the maximum ripple allowed.<br><br>3. Voltage stress (Rule #3):<br>For voltage stress derating above 67%, the expected life increases by the fifth power of the rated voltage (V_r)/applied voltage (V_a). | Derating greatly improves the life of the electrolytic capacitor in addition to ensuring greater protection from spikes that can cause shorted capacitors. |
| Detail: | |
| The expected life of a capacitor is described as the maximum expected life (Condition a) at rated temperature times the acceleration factors: temperature (T), voltage (V) and ripple current (I).<br><br>L_b = L_a × A_t × A_r × A_v<br><br>L_b = L_a × 2^((T_0 - T)/10) × 2 × (1 - (I/I_0)^2) × (V_r/V_a)^5<br><br>L_a = Lifetime under condition a, L_b = Lifetime under condition b, A_t = Temperature acceleration, A_v = Voltage acceleration, A_r = Ripple current acceleration. | |

## Figure 20.5 DFR guideline for electrolytic capacitor usage. Source: Courtesy of Teradyne, Inc.

## 20.4.1 Derating Guidelines

Derating guidelines are vital to any product development program and should be a part of the DFR guidelines. If there are no derating guidelines in place, chances are good that some of the customer failures are the result of stress levels that exceeded the component specifications. In addition, having sufficient derating in the design will result in increased design margin. Designs with sufficient design margin have lower test parts per million (PPM) failure rates and higher first-pass yields in production.

It is not necessary to develop derating guidelines from scratch. There are several derating guidelines in the industry that can be purchased. The guidelines cover a broad range of users and so they may not be usable by engineers in this form. However, it is a simple task to tailor derating guidelines for your specific application and user

environment. Derating guidelines are available from the Reliability Analysis Center. The ordering information is as follows:

Electronic Derating for Optimum Performance
Reliability Analysis Center
201 Mill Street
Rome, NY 13440-6916
http://rac.iitri.org/

Once the guidelines are in place, they can become part of the design review process. Derating should also be part of a design check off to verify compliance to the guideline prior to design review.

## 20.5 Design FMEA

The most powerful reliability tool in the design phase prior to building and testing any product is the FMEA tool. The design FMEA needs to be performed prior to signing off designs for procurement and build. The design FMEA activity takes place before any prototypes are ever built. If you’ve never performed FMEAs in your organization, this is one area where resistance is almost guaranteed. For some reason, designers have a hard time embracing the concept of a design FMEA. They will often point to the many design checks that are incorporated into the design process to eliminate errors as good enough, with “This is how we’ve always done it.” Typically, design reviews to catch problems include a checklist based on common mistakes made, peer reviews, automated simulation programs, and automated design check programs to verify compliance to design guidelines. These processes are valuable and necessary to the design process. However, they have limitations in the types of problems they can identify. Because the FMEA is a concurrent effort, potential reliability and safety issues can be identified and fixed where it will have the least impact on the program.

In Chapter 7, the FMEA process is discussed in detail. We recommend that the material presented in Chapter 7 be used to develop an FMEA training program. Prior to a design FMEA, it is imperative that all participants have been trained in the process. Performing design FMEAs for the first time in an organization is difficult. If the participants are not trained in the process, then the FMEA meeting will be highly unproductive. Untrained participants have a tendency to steer the team off on tangents and are more likely to challenge the process. It is better to delay the FMEA design review by a week so that everyone is trained than to train as you go through the design FMEA. The training doesn’t have to be long, but everyone needs to be familiar with the process.

FMEAs should be performed for all significant sections of the design. This includes the total system and subsystems including printed circuit board assemblies (PCBAs).

The best place to perform the design FMEA is after the design is completed but before the design goes through final design review. Design changes usually result from the FMEA, and so there is no point in doing the final design review until the FMEA items have been closed out. The benefits from a design FMEA include:

- Discovery of design errors
- Identification of system failures due to interconnects
