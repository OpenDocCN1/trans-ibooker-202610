
![](assets/cover.png)

![](assets/cover.png)

# Simulation Models for Data Science

by Dan Sullivan

Copyright © 2027 Dan Sullivan. All rights reserved.

Published by O’Reilly Media, Inc., 141 Stony Circle, Suite 195, Santa Rosa, CA 95401.

O’Reilly books may be purchased for educational, business, or sales promotional use. Online editions are also available for most titles ([https://oreilly.com](https://oreilly.com)). For more information, contact our corporate/institutional sales department: 800-998-9938 or *corporate@oreilly.com*.

* Acquisitions Editor: Aaron Black
* Development Editor: Jeff Bleiel
* Production Editor: Clare Laylock
* Cover Designer: Karen Montgomery
* Cover Illustrator: FILL IN
* Interior Designer: David Futato
* Interior Illustrator: Kate Dullea
* March 2027: First Edition

Add additional revdate spans below as needed.

# Revision History for the Early Release

* 2026-03-25: First Release

See [https://oreilly.com/catalog/errata.csp?isbn=9798341668324](https://oreilly.com/catalog/errata.csp?isbn=9798341668324) for release details.

The O’Reilly logo is a registered trademark of O’Reilly Media, Inc. *Simulation Models for Data Science*, the cover image, and related trade dress are trademarks of O’Reilly Media, Inc.

The views expressed in this work are those of the author and do not represent the publisher’s views. While the publisher and the author have used good faith efforts to ensure that the information and instructions contained in this work are accurate, the publisher and the author disclaim all responsibility for errors or omissions, including without limitation responsibility for damages resulting from the use of or reliance on this work. Use of the information and instructions contained in this work is at your own risk. If any code samples or other technology this work contains or describes is subject to open source licenses or the intellectual property rights of others, it is your responsibility to ensure that your use thereof complies with such licenses and/or rights. PROD: Uncomment the following sentence if appropriate and add it to the above para: This book is not intended as \[legal/medical/financial; use the appropriate reference\] advice. Please consult a qualified professional if you require \[legal/medical/financial\] advice.

979-8-341-66828-7

\[FILL IN\]

# Brief Table of Contents (*Not Yet Final*)

Chapter 1. Why Use Simulation When You Have Data?

Chapter 2. Building a Simulation Mindset: From Static Analysis to Dynamic Modeling

*Chapter 3. From Data to Distributions* (unavailable)

*Chapter 4. How Do I Model Risk and Uncertainty in Decision Making?* (unavailable)

*Chapter 5. How Do I Find and Fix Process Bottlenecks?* (unavailable)

*Chapter 6. How Do I Predict System Behavior with Feedback Effects?* (unavailable)

*Chapter 7. How Do I Model Market Dynamics and Competition?* (unavailable)

*Chapter 8. How Do I Create Digital Twins That Connect to the Real World?* (unavailable)

*Chapter 9. How Do I Deploy and Scale Simulations in Production?* (unavailable)

*Chapter 10. How Do I Ensure My Simulations are Trustworthy?* (unavailable)

*Chapter 11. How Do I Drive Adoption and Action from Simulation Insights?* (unavailable)

# Chapter 1. Why Use Simulation When You Have Data?

Data professionals have no shortage of tools, historical datasets, and predictive models, but we still struggle to understand complex business processes.

Consider supply chains. Small demand fluctuations can cascade into disproportionate order increases across retailers, distributors, and manufacturers. This is known as the bullwhip effect. This happens when each actor makes independent, rational decisions based on incomplete information, which can lead to system-wide disruptions.

Or consider capacity planning. Predictive models can fail to account for backlogs, randomness in arrival times, and phase transitions at critical thresholds where systems experience catastrophic gridlock.

Competitive pricing presents another challenge. Retailers can build price elasticity models based on historical data but cannot anticipate competitor reactions. This leaves them with static correlation analysis that offers little value for strategic decisions in dynamic markets.

Clearly, we are missing something in our data analytics and data science toolkit.

Simulation modeling fills those gaps in our suite of tools. A simulation model is an executable computer representation of a real-world system that defines the components of a system and describes rules that govern the interaction of those components. By considering both components and interactions, we can begin to understand how the system behaves over time and under different conditions. By modeling a large number of possible combinations of input parameters, we can reveal characteristics of novel situations that may not appear in historical data. By modeling components and interactions, we can discover feedback loops, time delays, and emergent properties that traditional statistical methods do not sufficiently model or predict.

Simulation modeling complements, it does not replace, statistical and machine learning techniques in data science. Statistical methods are essential for descriptive and predictive analysis, while machine learning methods help us identify patterns in large datasets. Simulation models are exploratory and forward looking. They allow us to identify causal relationships and develop more explanatory hypotheses for complex business processes.

This chapter begins with a quick look at why simulation is valuable today and how generative AI has revolutionized what data scientists can do. We then present an overview of complex business systems and why they are so hard to model and predict. We will look at three distinct business problems that highlight the challenges of analyzing complex systems.

Next, we will move on to look at the limits of traditional analytics methods. We’ll explore the benefits of adopting a “systems thinking” approach to understanding dynamic and complex systems.

Keeping in mind the aphorism that in theory, theory and practice are the same, but in practice they are not, we conclude the chapter with a case study. It describes a financial contagion that was not anticipated by traditional analytic tools but was elucidated by simulation modeling. This demonstrates the need for expanding our data science toolkit to include simulation.

## Why Simulation Now

If simulation is so useful, why haven’t data scientists and data analysts employed it more? And why should you be adopting it now?

There have been barriers to entry that have limited the adoption of simulation. Traditionally, simulation modeling required an understanding of specialized languages such as Simul8 and AnyLogic or the ability to code simulations in Python or R using specialized libraries. These are not the tools data professionals are used to working with, like SQL and Pandas. In addition, simulation modeling requires an understanding of probability distributions, statistical sampling methods, and queueing theory. Designing, building, and validating simulation could take a long time and can be hard to justify when using traditional simulation tools and software practices.

Generative AI and specification-driven development are fundamentally altering the simulation landscape. Large language models and large reasoning models are increasingly proficient at creating simulation code. Specification-driven development practices employ the rapid, exploratory nature of vibe coding but combine it with detailed requirements, technical architecture design, data models, API contracts, and security best practices.

The use of generative AI for simulation modeling is changing the scope of what data scientists can do. Today, a data scientist can use generative AI to help with the full lifecycle of simulation modeling, including:
