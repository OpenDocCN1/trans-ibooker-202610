![](assets/cover.png)

![](assets/cover.png)

# Simulation Models for Data Science

by Dan  Sullivan

Copyright © 2027 Dan Sullivan. All rights reserved.

Published by O’Reilly Media, Inc., 141 Stony Circle, Suite 195, Santa Rosa, CA 95401.

O’Reilly books may be purchased for educational, business, or sales promotional use. Online editions are also available for most titles ([https://oreilly.com](https://oreilly.com)). For more information, contact our corporate/institutional sales department: 800-998-9938 or *corporate@oreilly.com*.

*   Acquisitions Editor: Aaron Black
*   Development Editor: Jeff Bleiel
*   Production Editor: Clare Laylock
*   Cover Designer: Karen Montgomery
*   Cover Illustrator: FILL IN
*   Interior Designer: David Futato
*   Interior Illustrator: Kate Dullea

*   March 2027: First Edition

Add additional revdate spans below as needed. 

# Revision History for the Early Release

*   2026-03-25: First Release

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

# Why Simulation Now

If simulation is so useful, why haven’t data scientists and data analysts employed it more? And why should you be adopting it now?

There have been barriers to entry that have limited the adoption of simulation. Traditionally, simulation modeling required an understanding of specialized languages such as Simul8 and AnyLogic or the ability to code simulations in Python or R using specialized libraries. These are not the tools data professionals are used to working with, like SQL and Pandas. In addition, simulation modeling requires an understanding of probability distributions, statistical sampling methods, and queueing theory. Designing, building, and validating simulation could take a long time and can be hard to justify when using traditional simulation tools and software practices.

Generative AI and specification-driven development are fundamentally altering the simulation landscape. Large language models and large reasoning models are increasingly proficient at creating simulation code. Specification-driven development practices employ the rapid, exploratory nature of vibe coding but combine it with detailed requirements, technical architecture design, data models, API contracts, and security best practices.

The use of generative AI for simulation modeling is changing the scope of what data scientists can do. Today, a data scientist can use generative AI to help with the full lifecycle of simulation modeling, including:

*   Researching and defining components and interaction rules of a system
    
*   Choosing a simulation method, such as Monte Carlo, discrete event, system dynamics, or agent-based modeling
    
*   Generating standard simulation code for component creation, resource allocation, event scheduling, and data collection
    
*   Building statistical analysis scripts to understand the probability distributions of variables in the system to be modeled
    
*   Creating data engineering pipelines to extract data from source systems, apply needed transformations, and provide data to simulation models
    
*   Writing guides for end users as well as technical documentation, such as API documentation
    
*   Generating validation tests to measure the quality of the model
    
*   Defining appropriate ranges of sensitivity analysis tests
    
*   Creating visualizations and user interfaces for working with the simulation
    

Realistic simulations once required weeks of specialized training and months of development. Today we can use generative AI to help specify model requirements, understand fundamental mathematical principles, and generate production-quality code. This democratization does not eliminate the need for expertise and domain knowledge in complex simulation projects, but it reduces the barrier to entry for many use cases.

Spreadsheets democratized financial modeling and SQL democratized data analysis. Now AI-assisted simulation is making systems thinking and what-if analysis available to data professionals and software developers who can clearly specify their business problem, identify appropriate modeling techniques, and validate models. Simulation is no longer a specialized niche capability; it is a general-purpose tool that should be in every data professional’s toolkit.

This chapter begins with an overview of complex business systems and a discussion of why they are so hard to model and predict. We will look at three distinct business problems that highlight the challenges of analyzing complex systems: the bullwhip effect in supply chains, capacity planning, and the dynamics of competitive markets. Next, we will move on to look at the limits of traditional analytics methods, which includes dependence on historical patterns and the problem of novel situations, understanding correlation but not causation, using static snapshots to describe dynamic systems, focusing on central tendency without enough attention to extremes, and the risks of not understanding delays and latency in system dynamics. These issues motivate us to adopt a systems thinking approach to understanding dynamic and complex systems. Systems thinking employs several constructs including stocks and flows, feedback loops, delays and accumulations, as well as emergence and nonlinearity. Together, systems thinking and simulation modeling provide a powerful tool for analyzing problems with significant uncertainty, emergent properties, and the need to analyze novel, what-if scenarios. Keeping in mind the aphorism that in theory, theory and practice are the same, but in practice they are not, we conclude the chapter with a case study. It describes a financial contagion that was not anticipated by traditional analytic tools but was elucidated by simulation modeling. This demonstrates the need for expanding our data science toolkit to include simulation.

# Understanding Complex Business Systems

There are numerous tools and techniques for analyzing and understanding data. Sometimes, it is pretty obvious which tools should be used for a particular problem. For example, if you want to compare two groups of customers, you might start with descriptive statistics about each group, such as the average amount spent per customer and the number of customers in each group. If you want to predict customer churn, you might start with a baseline machine learning model based on logistic regression, which is fast to train, works with both categorical and numerical features, and outputs probabilities. Many times, we do not have such obvious solutions to our problems. Let’s consider three such scenarios a data scientist might encounter when working in logistics, healthcare, and retail.

## Supply Chain Bullwhip Effect

Let’s start with a simple question of how a change in one part of a supply chain can affect other parts of that supply chain. If a retailer decides to increase their average order size, it sounds reasonable to assume that change will lead to linear changes in other parts of the supply chain. That is often not what happens.

A retailer sells 100 units per day with a standard deviation of 20. One week, the retailer sells 110 units per day for one week, totaling 770 units for the week. That is 70 more units sold than expected. The retailer assumes the daily average is increasing and they adjust their order from the distributor to accommodate the new increased level plus replenish the inventory that was drawn down during the week of higher-than-expected orders.

If the retailer assumes the new level is 110 units per day, they may increase the inventory buffer they have on hand. This compounding of expected increase, replenishing stock, and increased inventory buffer might lead to a 30% increase in the number of products ordered from the distributor—from 700 units per week to 910 units per week.

When the distributor sees this jump in order size, they may assume the retailer will continue with the 30% increase level, triggering the distributor to increase their orders from manufacturers. The distributor increases orders from 2,800 units per month to 3,780 units per month (a 35% increase). The manufacturers see an increase in orders and follow the pattern of increasing their target production. Manufacturers have to order raw materials and they tend to have longer lead times than retailers or distributors, so they place a large order for materials that may be 50% higher than what they would have ordered if the original retailer orders had stayed constant—from 11,200 units per quarter to 16,800 units per quarter.

The amplification is striking: a 10% increase in retail demand cascades into a 50% increase in raw material orders. The amplification factor is 5× as shown in Figure 1-1.

![](assets/figure_1-1_bullwhip_cascade.png)

Now imagine what happens the following week when demand for the retailer’s products drops back to 100. The retailer has excess inventory, so it cancels some orders while it works down stock on hand. The distributor sees a significant drop in orders and cancels outstanding orders to manufacturers. The manufacturer sees a precipitous drop in orders and cancels some of its production plans.

The result of retailers, distributors, and manufacturers all making rational choices without full information about the state of the larger business system they operate in is highly disruptive. This phenomenon is known as the bullwhip effect in supply chains. It is caused by a combination of factors.

Each actor in this system has limited information. They act only on information about their orders, not the demand of the end consumer. When all of the actors in the system order weekly instead of continuously, we risk having synchronization effects that can amplify swings. Also, each actor reasonably decides to accumulate stock instead of risking not being able to meet demand. Manufacturers and others with long lead times to fulfill orders require larger inventory positions. These multiple factors interact to create an outcome that is difficult to predict unless we consider all the factors that influence the outcome.

Simulation is a tool that can help us better understand situations like the amplification phenomenon we see in the bullwhip effect. Simulation is especially helpful when we are dealing with:

*   Dynamic systems, such as groups of actors making different kinds of choices
    
*   Nonlinear impacts, such as when small changes in one part of a system trigger much larger changes in other parts of the system
    
*   Actors that maintain state information and make decisions based on state
    
*   Actors simultaneously making decisions with incomplete information
    
*   Multiple sources of uncertainty
    

Supply chains are not the only business process impacted by these phenomena.

## Capacity Planning: Averages Are Not Enough

Imagine you are a data scientist working for a healthcare company. You have been tasked with helping model capacity planning for an emergency room. You develop a model that predicts the average patient arrival by hour, day, and season. You have carefully researched variation in historical data and accounted for things like flu season, adverse weather conditions, and local high-traffic events. You also know how important it is to validate your model, so you use time series cross-validation and track metrics like mean absolute error (MAE), mean absolute percentage error (MAPE), and root mean square error (RMSE). Your model works well, with average daily prediction MAPE of 8% and hourly arrival prediction MAPE of 15%.

The validation results are promising, but how do they compare to reality? Let’s pick one time period, say Monday evenings 7 p.m. to 8 p.m., and see what one year of data looks like.

Figure 1-2 shows actual arrivals on Mondays from 7 p.m. to 8 p.m. for 52 weeks.

 
| Week || Arrivals || Week || Arrivals || Week || Arrivals || Week || Arrivals |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 || 12 || 14 || 9 || 27 || 11 || 40 || 8 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 2 || 11 || 15 || 13 || 28 || 10 || 41 || 12 |
| 3 || 10 || 16 || 8 || 29 || 14 || 42 || 9 |
| 4 || 13 || 17 || 12 || 30 || 9 || 43 || 11 |
| 5 || 9 || 18 || 10 || 31 || 13 || 44 || 10 |
| 6 || 11 || 19 || 11 || 32 || 8 || 45 || 13 |
| 7 || 8 || 20 || 9 || 33 || 12 || 46 || 9 |
| 8 || 12 || 21 || 13 || 34 || 10 || 47 || 11 |
| 9 || 10 || 22 || 8 || 35 || 11 || 48 || 8 |
| 10 || 14 || 23 || 12 || 36 || 9 || 49 || 12 |
| 11 || 9 || 24 || 10 || 37 || 13 || 50 || 10 |
| 12 || 11 || 25 || 11 || 38 || 8 || 51 || 11 |
| 13 || 10 || 26 || 9 || 39 || 12 || 52 || 9 |

![](assets/figure_1-2_er_distribution.png)

Your model predicts an average of 10.5 patients per hour during this time slot. The average of the actual data is 10.5 patients per hour. Your model is accurate on average. The problem is that “on average” does not tell the full story.

During 52 weeks of the year, the emergency room had 10 or 11 patients 32 times (62% of weeks). It had 8 or 9 patients 15 times (29% of weeks) and 12, 13, or 14 patients 5 times (10% of weeks). If the emergency room staffs for the average, they will be understaffed 10% of the time and overstaffed 29% of the time.

This is where understanding variability becomes critical. The standard deviation of arrivals is 1.7 patients. If we want to ensure adequate staffing 90% of the time, we need to plan for approximately 12.7 patients per hour (mean + 1.3 standard deviations). If we want 95% coverage, we need to plan for 13.3 patients (mean + 1.65 standard deviations).

But arrival rates are only part of the story. Service times also vary. If the average service time is 45 minutes with a standard deviation of 15 minutes, and we have two doctors on duty, queueing theory tells us the system utilization is:

> Utilization = (Arrival rate × Service time) / Number of servers = (10.5 × 0.75) / 2 = 0.39 or 39% 

![](assets/figure_1-3_queueing_utilization.png)

At 39% utilization, average wait times are manageable. But what happens when arrivals spike to 14 patients per hour? Utilization jumps to 52.5%, and average wait times more than double due to nonlinear queueing effects. At 80% utilization, the system approaches gridlock, and wait times can exceed several hours.

Simulations can help us identify the full range of variability so we can address different multiple likely scenarios rather than focus on only the average scenario.

## Competitive Market Dynamics

Competitive dynamics present another challenge for traditional analytical methods. Consider a retail pricing problem. Two competing stores sell similar products. You have historical data on your own sales at different price points and have built a price elasticity model. Your model suggests that dropping your price by 10% will increase sales volume by 18%, leading to a 6% increase in gross revenue.

You implement the price reduction and watch your competitor. Within a week, they match your price cut. Your sales volume increases by only 9% instead of 18%, and gross revenue drops by 2%. Your model failed to account for competitive response.

Let’s quantify this with a simple example. Suppose both stores currently sell 1,000 units per week at $50 per unit, generating $50,000 in weekly revenue. Here’s what happens under different scenarios:

*   **Baseline**
    
    *   Your Price: $50
        
    *   Competitor Price: $50
        
    *   Your Volume: 1,000
        
    *   Competitor Volume: 1,000
        
    *   Your Revenue: $50,000
        
    *   Competitor Revenue: $50,000
        
*   **You cut, they hold**
    
    *   Your Price: $45
        
    *   Competitor Price: $50
        
    *   Your Volume: 1,800
        
    *   Competitor Volume: 600
        
    *   Your Revenue: $81,000
        
    *   Competitor Revenue: $30,000
        
*   **You cut, they match**
    
    *   Your Price: $45
        
    *   Competitor Price: $45
        
    *   Your Volume: 1,090
        
    *   Competitor Volume: 1,090
        
    *   Your Revenue: $49,050
        
    *   Competitor Revenue: $49,050
        
*   **Both increase**
    
    *   Your Price: $55
        
    *   Competitor Price: $55
        
    *   Your Volume: 850
        
    *   Competitor Volume: 850
        
    *   Your Revenue: $46,750
        
    *   Competitor Revenue: $46,750
        

The “You cut, they match” scenario illustrates what happened in reality. The competitor’s rational response was to match your price cut to prevent losing market share. The result is both companies earn less revenue at the lower price point. Simulation modeling can help us identify the effects of responses to our actions and provide a more realistic picture of likely outcomes.

# The Limits of Traditional Analytics

Why do traditional data science methods struggle with these business problems? The limitations fall into five categories: dependence on historical patterns, confusion of correlation with causation, static snapshots of dynamic systems, focus on central tendency without attention to extremes, and inadequate treatment of delays and latency.

## Dependence on Historical Patterns and the Problem of Novel Situations

Machine learning models are trained on historical data. They learn patterns from the past and assume those patterns will continue into the future. This works well when the future resembles the past. It fails when conditions change.

Consider the capacity planning example. If we train a model on historical emergency room arrival data, it will predict average arrival rates accurately. But what if a new urgent care center opens nearby? Or a major employer closes, changing the demographics of the area? Or a pandemic changes patterns of healthcare-seeking behavior? Historical patterns may no longer apply.

The bullwhip effect example illustrates another limitation. Historical data may show stable demand patterns because the supply chain has been in equilibrium. But what happens when demand suddenly spikes? Without historical examples of large demand shocks, predictive models cannot anticipate the amplification effect.

Simulation models do not depend solely on historical patterns. They model the underlying mechanisms that generate outcomes. By representing components and interaction rules, simulation can explore scenarios that have never occurred historically.

## Understanding Correlation but Not Causation

Most predictive models identify correlations in data. These correlations can be highly predictive but may not reflect causal relationships. Without understanding causation, we cannot reliably predict the effects of interventions.

In the pricing example, historical data shows a correlation between your prices and your sales volume. But the correlation does not reveal how competitors will respond. A causal model would need to represent:

*   How customers perceive value and make purchase decisions
    
*   How competitors monitor market conditions and decide whether to respond
    
*   How market share shifts based on relative pricing
    
*   How these dynamics unfold over time
    

Traditional regression or machine learning models treat these as exogenous variables or ignore them entirely. Simulation models can explicitly represent causal mechanisms.

This distinction matters because correlation-based models fail when we intervene in the system. A model that predicts sales based on historical correlations between price and volume may work for forecasting, but it cannot reliably predict the effect of a planned price change when competitors will respond.

## Static Snapshots to Describe Dynamic Systems

Many analytical methods take a snapshot view of systems. They analyze data at a point in time or aggregate data across time periods, losing information about dynamics.

Consider the emergency room capacity planning problem. A cross-sectional analysis might calculate average daily patient volume and average service time, then recommend staffing levels based on those averages. This approach misses:

*   How patient backlogs accumulate during busy periods
    
*   How service times increase when staff are overwhelmed
    
*   How the queue clears during slower periods
    
*   How daily or weekly cycles create predictable patterns of congestion
    

Dynamic systems have memory. What happens at time t depends not only on conditions at time t but also on what happened at t-1, t-2, and earlier. Backlogs accumulate, feedback loops operate, and systems can exhibit hysteresis where the current state depends on the path taken to reach it.

Simulation models are inherently dynamic. They advance through time, updating system state at each time step. This allows us to observe how systems evolve and how interventions affect trajectories.

## Focusing on Central Tendency Without Enough Attention to Extremes

Most statistical methods focus on means, medians, and other measures of central tendency. This is appropriate for many applications but can be misleading when extremes matter.

![](assets/figure_1-4_distribution_comparison.png)

In the emergency room example, planning for average arrival rates leaves the system understaffed during peak periods. Patients experience long waits, staff become overwhelmed, and quality of care suffers. Understanding the distribution of arrivals and service times is as important as understanding the mean.

Risk management requires understanding tail events. In supply chains, the cost of stockouts during demand spikes may far exceed the cost of holding extra inventory. In financial systems, rare but severe events like market crashes or bank failures can destroy more value than years of normal returns create.

Traditional methods often assume normal distributions or rely on measures like standard deviation that may not fully characterize tail behavior. Fat-tailed distributions, where extreme events are more likely than a normal distribution would suggest, are common in business contexts but poorly captured by methods that focus on central tendency.

Simulation models can generate full probability distributions of outcomes. By running thousands of scenarios with varying inputs, we can understand the range of possible outcomes, identify extreme cases, and quantify tail risks.

## Not Understanding Delays and Latency in System Dynamics

Many business processes involve delays. Orders take time to fulfill. Customers take time to respond to price changes. Manufacturing capacity takes time to add. Information takes time to propagate through organizations.

Delays create dynamics that are difficult to analyze with static methods. In the bullwhip effect example, the long lead time for raw materials causes manufacturers to over-order when they perceive demand increases. The delay amplifies the cycle because manufacturers cannot quickly adjust when demand falls.

Delays can create oscillations, where systems repeatedly overshoot and undershoot equilibrium. They can create instability, where small perturbations grow over time. They can create phase shifts, where systems transition from one regime to another.

Simulation models explicitly represent delays. We can model information delays, transportation delays, production delays, and decision-making delays. By observing how these delays affect system behavior, we can design better policies and identify interventions that dampen oscillations or prevent instability.

# Systems Thinking: A Framework for Complex Systems

The limitations of traditional analytics motivate a different approach to understanding complex business systems. Systems thinking provides a conceptual framework for reasoning about dynamics, feedback, and emergence. Simulation provides the computational tools to implement and test systems thinking models.

Systems thinking employs several key constructs: stocks and flows, feedback loops, delays and accumulations, emergence and nonlinearity, and what-if scenarios.

## Stocks and Flows

Stocks are accumulations. They represent quantities that build up or drain over time. In a supply chain, inventory is a stock. In healthcare, the backlog of patients waiting for service is a stock. In finance, account balances are stocks.

Flows are rates of change. They represent how quickly stocks increase or decrease. Customer orders are a flow that increases retailer inventory. Sales are a flow that decreases inventory. Patient arrivals increase the backlog, while patient service decreases it.

The fundamental relationship between stocks and flows is:

> Stock(t) = Stock(t-1) + Inflow(t) - Outflow(t) 

![](assets/figure_1-5_stock_flow_diagram.png)

This simple equation captures how systems accumulate or deplete resources over time. When inflows exceed outflows, stocks grow. When outflows exceed inflows, stocks shrink. When they balance, the system is in equilibrium.

Many business problems involve understanding how stocks respond to changes in flows. If a manufacturer increases production (inflow) faster than customers buy products (outflow), finished goods inventory (stock) accumulates. If an emergency room experiences a spike in patient arrivals (inflow) while service capacity (outflow) remains constant, the waiting room backlog (stock) grows.

Simulations allows us to model stocks and flows explicitly and observe how they evolve under different conditions. A spreadsheet model can help us understand the stock-flow equation at a single point in time but it can be hard to model how inflows and outflows change dynamically based on the state of the system. A simulation model lets us define the rules governing flows and then observe how stocks behave over weeks, months, or years. This is especially useful when multiple stocks are interconnected, such as when raw material inventory, work-in-progress, and finished goods all interact through shared flows.

## Feedback Loops

Feedback loops occur when the output of a system influences its own input. Positive feedback amplifies changes. Negative feedback dampens changes and promotes stability.

![](assets/figure_1-6_positive_and_negative_feedback_loops.png)

The bullwhip effect is an example of positive feedback. When retailers see increased demand, they increase orders to distributors. Distributors see increased orders and amplify their orders to manufacturers. The system overshoots demand, creating excess inventory. When demand falls, the feedback operates in reverse, creating stockouts.

Negative feedback creates stability. A thermostat is a classic example. When temperature rises above the set point, the thermostat turns off heating. When temperature falls below the set point, it turns on heating. The feedback loop maintains temperature near the set point.

Business systems often contain both positive and negative feedback loops operating simultaneously. Positive feedback can drive growth or decline. Negative feedback can create stability or resistance to change. The interaction of multiple feedback loops can create complex dynamics that are difficult to predict without modeling.

Simulation models make feedback structure explicit and computable. We can observe how a change propagates through the system, identify which feedback loop dominates at any given time, and discover tipping points where dominance shifts from one loop to another. The behavior of a system is determined by its feedback structure and simulations help us make that structure visible.

## Delays and Accumulations

Delays are ubiquitous in business systems. Manufacturing takes time. Shipping takes time. Customers take time to respond to marketing. Organizations take time to recognize problems and implement solutions.

Delays interact with feedback loops to create dynamics. Long delays in negative feedback loops can cause oscillations. If a thermostat has a long delay between sensing temperature and adjusting heating, the system will overshoot and undershoot the set point repeatedly.

Accumulations occur when flows persist over time. Small daily deposits accumulate into large balances. Small daily patient arrivals in excess of service capacity accumulate into large backlogs. Small daily quality defects accumulate into large rework costs.

The combination of delays and accumulations creates many business problems. Inventory accumulates because of delays in adjusting production to match demand. Backlogs accumulate because of delays in adding capacity to match workload. Technical debt accumulates because of delays in refactoring code.

Delays and accumulations are difficult to manage with intuition. We might take an action and not see an immediate results so we take an additional action, which can lead to overcorrecting because the original intervention didn’t have time to take effect. Simulation models make delays explicit by tracking the time between cause and effect. We can experiment with different delay lengths to understand their impact on system stability, test policies designed to compensate for known delays, and identify which delays in a system are most critical to reduce.

## Emergent Properties

When components of a system interact, we may see patterns of behavior that are not easily predicted just from looking at rules of interaction. Take the case of bird flocking. Birds tend to follow a few simple rules like stay close to other birds, avoid collisions, and match the speed of other birds. There is no centralized coordination, but complex patterns like murmurations, where birds dive, swirl, and turn together, can emerge.

Many analytic methods are fundamentally reductionist; they look at individual components and miss how properties emerge from interactions. It is difficult to discover phase transitions with such methods, especially if historical data does not have examples of possible emergent patterns.

Simulation models help here by defining components and the rules for their interaction and then allowing us to observe how the system behaves over time. Observing components as they interact allows us to see macro-level patterns that are only apparent when components interact over time. This has wide applicability in data science, such as understanding bullwhip amplification in supply chains, queueing dynamics and phase transitions in systems where work accumulates, and financial contagion like spreading bank failures during financial crises. Financial contagion is the spread of financial distress from one institution, market, or country to others.

## What-If Scenarios

Simulations are particularly useful when we want to reason about novel situations. By definition, novel situations have not been seen before, so we have no directly applicable historical data to guide us. By creating a simulation, we can produce a model of a system and test changes to the inputs of the system and alter rules of interactions to understand the impact of those changes. Rather than do experiments in our businesses or organizations, we can simulate:

*   Adding customer service agents to assist customers to understand the impact on gross sales revenues
    
*   Extending open hours of stores by varying amounts to find the optimal number of hours to be open
    
*   Delayed delivery of products to understand the impact on store revenue
    
*   Combining multiple changes, like increasing the number of customer assistants and extending hours
    

Simulations are particularly useful when we want to reason about novel situations. Statistical and machine learning models are trained on historical data, so they are most reliable when the future resembles the past. But strategic decisions, like deciding to enter a new market, restructure a supply chain, or change pricing are situations where the future will definitely differ from the past. Simulation models the mechanism of change rather than identifying a pattern, allowing us to explore conditions beyond what is captured in historical data.

# Case Study: Predicting Financial Contagion in Banking Networks

In 2008 the global economy experienced a sudden downturn triggered by a collapse in the United States housing market. A build up in housing values coupled with risky subprime mortgages created a market bubble. When the bubble collapsed, housing values fell and many homeowners defaulted on loans. The impact of those defaults was not limited to the banks that made the original loans, however. This is because financial institutions bundled groups of mortgages into securities that were sold to investors. This spread the risk of default to a much broader set of institutions and investors.

The value of these securities fell as defaults increased. This was a problem for banks and other institutions that held these securities and had to comply with an accounting method known as marking to market. Basically, they had to report the value of their assets, including mortgage backed securities, at their current market value and not the original value when the asset was acquired. In 2008, this resulted in many banks reporting losses and insufficient capital to meet regulatory requirements. As a result, these banks faced additonal regulatory restrictions, interventons by regulators, and in some cases a loss in confidence that could lead to a bank run.

Bank crisises occurred prior to 2008 and regulators had tools for assessing the resilience of financial institutions but those tools did not identify the lack of resilience in the financial system in 2008. A study by Prasanna Gai and Sujit Kapadia for the Bank of England in 2010 used simulation modeling to understand the components and interactions of the financial system that led to a state of low resilience and risk of financial contagion. Gai and Kapadia’s study is described in [“Contagion in financial networks”](https://www.financialresearch.gov/working-papers/files/OFRwp-2015-21_Contagion-in-Financial-Networks.pdf).

The simulation is based on complex network theory. It models banks and relationships between banks in terms of claims and obligations. For example, if Bank A lends money to Bank B, then Bank A has a claim on Bank B and Bank B has an obligation to Bank A. The links between banks represent claims and obligations and those links are weighted according to the based on the value of the claim or obligation.

In the terminology of network theory, each bank can be described in terms of two degree properties: the in-degree or the number of incoming links representing assets owed to the bank, and out-degree or the number of outgoing links representing liabilities to other banks.

In addition to modeling relationships between banks, the study authors also modeled properties of each bank. These included the banks assets and liabilities. . Assets were broken down into two type: interbank assets, which are claims on other banks, and illiquid external assets, such as mortgages. Liabilities also come in two forms: liabilities owed to other banks and customer deposits, which are funds owed to customers.

The capital buffer of a bank is the difference between the value of the bank’s assets and liabilities. A bank is considered solvent if as long as its assets are greater than its liabilities. A bank fails when its losses on interbank assets plus any lost value on mortgages exceed its capital buffer. The authors also defined a bank as vulnerable when it did not have sufficien capital buffer to cover the loss if one of its neighboring banks defaulted and could not pay its obligations to the bank.

The authors built a benchmark network of 1,000 banks and randomly defined links between banks. (They also modeled 10,000 banks but the increase in nodes did not change the overall results of the study). They then ran simulations that started with all banks solvent, and then introduced an initial shock. There were two types of shocks. One was a single bank failure. In this study, a bank failure was modeled as the loss of all of a banks assets. The authors also modeled aggregate shocks, in which all banks experience a loss in value of external assets, like mortgage backed securities. This second type of shock modeled the impact of marking asset values to their current market value at times when the value of those assets are dropping in general across the market.

The model simulates financial contagion through the network using two transmission methods: direct exposure to a failing bank and indirect contagion through a loss of liquidity in assets held by the bank.

When a bank fails, such as at the start of a simulation, then all of its neighboring banks that are its creditors incur a loss. The creditor banks loss all of the value of the interbank assets held against the defaulting bank. Now, the creditor banks have to update their capital buffer to reflect this loss. If their capital buffer is not sufficient to cover this loss, then they too become a failed bank. Failures in the creditor banks trigger another round of contagion possibly leading to other bank failures. The process continues until no new banks are sent into default.

![](assets/figure_1-7_financial_contagion_diagram.png)

The model also captures the impact of asset-related contagion in which a default leads the defaulting bank to sell its assets. The sudden sales of large volumes of an asset, known as a fire sale, can cause the price to drop. When that happens, all banks have to mark to market and adjust the value of their capital buffer. If their buffer drops too much, they become vulnerable.

# The Results

Running multiple simulations, the authors were able to identify a number of important characteristics of how financial contagion spread in the network.

The most imporant finding is that financial networks exhibit a “robust-but-fragile tendency.” In highly connected financial networks, a large number of links from a failed bank allow the losses to be distributed to more institutions. This provides risk-sharing benefits and leads to a low probability of contagion. At the same time though, a high number of links increases the potential for contagion to spread. This leads to fragility in the network. The probability of a contagion peaks at intermediate levels of connectivity before falling off. The risk of contagion continues to increase as the interconnectedness of the network increases. This is because banks are exposed to multiple defaults of interconnected banks, an increase chance of vulnerability to second-round defaults which can further deplete capital buffers.

The overall result is that high connectivity lowers the probability of contagion because losses are widely shared, but it also dramatically increases the severity of default event if a cascade begins, because banks are linked to so many others, making them susceptible to multiple hits during the contagion process.

![](assets/figure_1-8_robust_fragile_paradox.png)

In this study, simulation allowed researchers to clarify and quantify the complex, non-linear dynamics of contagion in a realistic network model. For example, is showed that probability of contagion is not monotonically related to connectivity and instead, the probability peaks at intermediate levels of connectivity and then drops off as connectivity increases. This reflects the impact of both the spreading of risk while increasing the routes by which contagion can spread. Overall, the use of simulation allowed the authors to generate quantitative and testable model results that captured some of the non-linear, high-complexity processes at work in financial networks.

## Comparison of Modeling Approaches

Different modeling approaches have different strengths and weaknesses. Table 1-1 compares statistical models, machine learning approaches, and simulation modeling across several dimensions to help you choose the right tool for your specific problem. Statistical and machine learning methods excel at analyzing historical patterns and predicting known outcomes. Simulation modeling is well suited to exploring novel scenarios, revealing causal mechanisms, and discovering emergent phenomena like tipping points and cascading dynamics.

 
|  || Traditional Statistical Methods || Machine Learning Approaches || Simulation Modeling |
| --- | --- | --- | --- |
| **Description** || Regression analysis, correlation studies, time series models (ARIMA, VAR), probability distributions from historical data || Neural networks, random forests, support vector machines, deep learning models trained on historical patterns || Agent-based models, network simulations, Monte Carlo methods that recreate system dynamics and test scenarios |
| --- | --- | --- | --- |
| **Data Requirements** || Historical failure data; Interbank exposures; Financial time series; Regulatory reports || Large training datasets; Labeled examples of failures; Multiple crisis episodes; Feature engineering data || Network structure; Balance sheet data; Behavioral rules/assumptions; Estimating parameters |
| **Key Strengths** || Well-established theory; Interpretable results; Clear assumptions; Fast computation; Confidence intervals; Hypothesis testing || Captures complex patterns; Non-linear relationships; Handles big data well; Scales with data || Tests “what-if” scenarios; Explores novel situations; Reveals causal mechanisms; No historical data needed; Models emergence; Experiments possible |
| **Key Limitations** || Assumes future is like past; Rare events not adequately represented; Linear relationships; Misses emergent behavior; Cannot test new scenarios; Correlations without insights about causation || “Black box” decisions; Needs massive data; Overfitting risk; Poor on rare events; Cannot extrapolate well; Doesn’t explain “why” || Model uncertainty; Parameter estimation; Computational cost; Validation challenges; Requires domain expertise; Simplified assumptions |
| **Best Use Cases** || Analyzing historical patterns; Testing specific hypotheses; Measuring correlations; Short-term forecasting; When data is abundant || Pattern recognition tasks; Predicting known outcomes; Large dataset analysis; Real-time risk scoring; Classification problems || Novel scenarios; Policy experiments; Understanding mechanisms; Rare event analysis; Network structure effects |
| **What It Can Reveal** || Historical contagion rates; Correlation of defaults; Geographic clustering || Likelihood of default; Risk classifications; Early warning signals || Robust-yet-fragile paradox; Tipping points; Network vulnerabilities; Connectivity thresholds; Cascade dynamics; Intervention effects |
| **What It Cannot Reveal** || Phase transitions; Network structure effects; Non-linear thresholds || Robust-yet-fragile paradox; Optimal connectivity; Causal pathways || N/A - Best suited for discovering novel insights |

# Summary

Simulation modeling complements traditional data science techniques by enabling analysis of complex business systems that exhibit emergent behaviors, feedback loops, and phase transitions. There is no question that statistical methods are essential for descriptive analysis and machine learning excels at identifying patterns in historical data. Simulation adds other important capabilities to the data science toolbox. It allows data professionals to explore forward-looking, what-if scenarios that have never occurred.

Traditional analytics approaches are not always sufficient for understanding complex systems because they rely on historical patterns, identify correlation but not causation, and provide static snapshots of dynamic systems. They also risk focusing on central tendencies rather than extremes and can fail to account for delays and latency in dynamic processes. The bullwhip effect in supply chains, capacity planning challenges in healthcare, and competitive pricing dynamics all demonstrate how small changes can cascade into disproportionate system-wide effects that traditional methods cannot readily predict.

The chapter introduced systems thinking as a framework for understanding these complex dynamics through stocks and flows, feedback loops, delays and accumulations, as well as emergence and nonlinearity. The Bank of England financial contagion study validates simulation’s effectiveness, showing the counterintuitive “robust-yet-fragile” property where moderate network connectivity proves more dangerous than high connectivity. Simulation complements rather than replaces existing methods, providing an essential tool for exploratory analysis when understanding causal mechanisms and testing novel scenarios.

# Chapter 2. Building a Simulation Mindset: From Static Analysis to Dynamic Modeling

This book is motivated by a single conjecture: data scientists need a variety of tools that enable both retrospective analytics and forward-looking simulation. You might justifibly ask, why? Afterall data science has been around as a named discipline for at least 25 years and it’s roots go back even further into the history of statistics, computational thinking, machine learning, and database systems. Those foundational disciplines also contribute to simulation modeling so it is best to think of simulation modeling as an aspect of data science, along with statitics and machine learning. Simulation modeling is the set of practices and methods that help us address questions that traditional analytics with its retrspecitve approach are not well suited for.

In this chapter, we’ll start with a review of the main challenges facing data scientists that are difficult to analyze with traditional analytics, such as descriptive statistics, diagnostic analytics, and predictive analytics/machine learning. (See Table 2.1 Traditional Analytics vs Simulation Modeling). We then examine ways to identify simulation opportunities in your organization. These include identifying early warning signs that traditional analytics are not appropriate, the feasibility of successfully developing a simulation model, and tips on building a business case for simulation. The chapter concludes with a look at the technology stack you use when building simulations, including Python and simulation related libraries, large language and large reasoning models, a specification-driven development methodology, production deployment and operations infrastructure, as well as tips on integrating simulation modeling into existing data engineering pipelines.

Table 2-1. Traditional Analytics vs. Simulation Modeling

  
| Question Type || Traditional Analytics || Simulation Modeling |
| --- | --- | --- |
| What happened? || ✓ Descriptive stats || Not needed |
| --- | --- | --- |
| Why did it happen? || ✓ Regression, ML || ✓ Causal mechanisms |
| What will happen? || ✓ Forecasting || ✓ Dynamic projection |
| What if X changes? || Limited (assumes linear) || ✓ Full scenario exploration |
| What’s optimal? || ✓ Static optimization || ✓ Dynamic optimization |
| What emerges? || ✗ Cannot detect || ✓ Reveals emergent behavior |

# What Is Challenging to Analyze with Traditional Analytics?

It would be easier to justify the need for simulation modeling if there were just one or two broad areas where traditional analytics are insufficient. The limits of traditional analytics with respect to the problems we need to solve as data scienticts is more of a hodge podge collection of system characteristics that don’t fit well with the assumptions underlying traditional analtyics. These include:

*   Feedback loops
    
*   Delays
    
*   Non-linear effects and thresholds
    
*   Capacity contsraints and resource contention
    
*   Uncertainty and risk propogation
    
*   Adaptive and strategic behavior
    
*   Path dependence and historical contingency
    

Rather than try to find some overly abstract principle that encompasses all of these, let’s dive right in to each of these issues individually.

## Feedback Loops

A feedback loop is a process in which the outcomes of some action influences the outcome of a future action. Feeback loops are common in biology and help organisms maintain healthy states but they are also common in business.

In Chapter 1, we discussed the bull whip effect in supply chains in which each actor in a supply chain based orders on the demand for their products and curent inventory levels. Small changes at the retail level propogated through the supply chain eventually creating oscillations in ordering patterns. Another well known feedback look in business is known as the Death Spiral in the health insurance industry.

A death spiral in a healthcare market starts when insurance carriers increase premiums. When that happens, price sensitive, healthy customers drop their insurance or find less expensive alternatives. This leaves a pool of insured persons who are, on average, sicker than the pool that included the recently churned healthy customers. Now the insurer is left with customers making, on average, higher health care claims and driving up costs. Those higher costs drive up insurance premiums that in turn drive other customers away.

![](assets/figure_2-1_insurance_death_spiral.png)

If we used a traditional analytic technique like linear regression, we could come up with a model that predicts a percent decrease in customers based on a percent increase in premiums. For example, we might look at historical data and estimate that a 10% increase in premiums lead to a 5% decreaes in the number of paying customers. This predicition may work well for the first round of customers dropping insurance but after the second increase in premiums, we might have a 8% rate of customer loss. Assuming that triggers another round of premium increases, we may see 12% customer loss. This process continues with accelerating incraeses in the rate of customer loss.

Simulation is a better approach to understanding the nature of the dealth spiral. It allows us to model how customer make decisions about cost vs risk tolerance as well as modeling the changing compositon of the pool of insured and how that affects costs. Simulations can help us identify thresholds, such as when the death spiral becomes irreversible becaue the rate of customer loss is so high. We can also use simulations to test different assumptions about price sensitivity and introduce new product features, like wellness incentives that can influence the cost dynamics for the insurer.

Another example of a feedback loop occurs with marketing campaigns. Changes in the way a retailer uses different marketing channels may show initial success. We may measure a 5% increase in sales revenues and rightly assume the new marketing strategy is the cause. If we stop at that level of analysis we’ll miss the response of our competitors. If, instead of measuring the change in revenue at one point in time, we built a model based on system dynamics, we could get a better understanding of how the makerting changes create unanticipated outcomes through reinforcing loops, such as competitors dropping thier prices.

## Delays

Simulation modeling helps us understand complete business systems rather than just a component within in a larger system. The immediate impact of a change in aa point of sale systems might be obvious: we reduce the number of abandonded carts for an online retailer. To understand the full impact of the change we should understand how it affects other components and aspects of the wider business system. For this, we need a way to model outcomes that unfold over time and across different componets of a system.

Let’s look at a basic A/B test for an online retailer. The retailer has streamlined the checkout process to reduce the number of pages a customer has to click through to complete a purchase. After a couple of weeks, the retailer finds that there is a 15% increase in the number of compeleted checkouts relative to the number of checkouts started. Great, the retailer takes the win and continues to use the streamlined checkout process.

A couple of months after the change, the retailer notices an increase in the return rate. Prior to the check out change, the commpany had a 5% retrun rate. Since the change, that rate has been increasing and is now approaching 10%. The retailer starts analyzing data about sales before and after the change and finds differences in the two populations (before and after change). After the change, there is an increase in the number of incorrect shipping addresses and wrong order quantities. This leads to higher labor costs to process returns, increased shipping costs, as well as lower customer satisfaction. Eventually, the retailer will see customer ratings decline and eventually customer churn and a loss in net promoter score.

## Non-linear Effects and Thresholds

A tell tale sign of a complex system is a sudden change in the state of the system that is not predicted by past behavior. As water cools, it maintains its liquid state until it reaches the freezing point and suddenly becomes solid. There is no linear progression from a more liquid state to a more solid state, there is a sudden transition. These transitions occur in a variety of systems.

### Sudden Phase Transitions at Critical Points

Queueing systems are widely used to model processes in which a request for a service arrives, possibly waits, receives a service, and then departs. If requests arrived at regular 10 minute intervals and always 9 minutes to process then no waiting would occur. When there is variability in the time requests arrive or in the time required to process, then queues will occur. Of course, we could scale our processing so that no matter how frequent the requests are, we can keep up. This kind of peak-demand sizing is wasteful and inefficient. The math behind queueing indicates that as queue lengths approach 80-85% utilization, we can expect a sudden increase in processing time, regardless of the the historical average of processing time was.

![](assets/figure_2-2_queue_length_vs_utilization.png)

### Diminishing and Accelerating Returns

Linear thinking comes naturally. If we increase our workforce by 10%, we’ll increase our output by 10%. With the help of statistics, we can often find factors, like revenue per salesperson or cost per unit produced. Combine these metrics with some basic spreadsheet skills and we can make all kinds of linear projections. This kind of reassoning sometimes works out when we restrict ourselves to narrow ranges and ideal circumstances. In real world scenarios with all their comlexity we often find relationships between inputs and outputs have non-linear relationships.

Consider the cost of acquireing new customers. A new direct-to-consumer retailer has been advertising for six months. Based on the initial data, the company determines that the average cost to acquire a customer is $50. Executives approve a budget of $250,000 for customer acquistion with the expectation that this will increase the number of customers by 5,000. After 2 months and an exhausted budget, the company has acquired and additional 2,800 customers. What they failed to appreciate is that the early customers where the easiest to acquire. They were the ones most interested and engaged with the company while the customers acquired later were less responsive to advertisments.

### Complex Interactions between Resources

When we need multiple resources to produce some good, it can be challenging to optimize the allocation of those resources. We might think, adding 10% more of one resource will increase our output by 10% (there is that linear thinking again). Adding 10% might increase the output of the one component but how does that increase affect other components? If a manufacturer has 10 produciton lines and adds one more, they will likely get 10% increase in the number of units produced. That does not seem unreasonable. The problem is that we stop thinking about the process too soon. What happens to those additional 10% of units produced? If they are sent to a quality control inspection station, then we risks creating a backlog of work for inspectors and this in turn creates a queue of work to be done.

### Emergent Properties from Component Interactions

Emergence is one of the most challenging aspects of working with complex systems. Emergence occurs when a behavior or characterisitc appears in a system that could not be predicted by analyzing the patterns of interaction of system components. When emergence occurs, a system is not simply the sum of its parts, it is something more. This phenomenon is ever present in biology. Cardiologist can explain how a heart muscle works in terms of responding when a neighboring cell exceeds a certain voltage, causing the cell to contract, enter a period where it cannot fire (refactory period) and then recover. Millions of cells in our hearts establish a dominant rhythm that synchronizes the firing process at the organ level in spite of the fact that individual cells have different firing rates. Researchers have found that two hearts with identical cellular properties can have different deviations from normal firing based on characteristics of tissue and prior states of the organ. To understand firing at the organ level, we have to reason about collections of cells or tissues because that is where the behaviors we are interested in emerge.

## Capacity Constraints and Resource Contention

The past is not always a good indicator of the future, and this is especially true when comparing a system during normal operations and the same system under stress. If we try to generalize principles from normal times that apply during high-stress periods, we are likely to miss the need to make trade-offs as resources become constrained.

Hard limits that invalidate extrapolation
: 

Capacity boundries are hard limits. No matter how hard we try, a machine that produces at most 20 units per minute will not produce 30 units per minute not matter what a linear projection says. Too often, we use linear models fit to historical data that assume the trend can continue indefinetly. In practice, as we approach capacity constraints, we hit dicontinuities that disrupt that the process and cause a sudden change in behavior of the system. For example, a shipping company can process 15,000 to 25,000 packages per shift. The more packages they ship, the longer it takes to ship each package. A linear model from historical data finds a 0.2-hour increase per 1,000 additional daily packages. One day during peak holiday season, the company processes 32,000 packages in one day. The predicated delivery time is 8.6 hours but in fact the actual delviery time was closer to 15 hours. The problem is that facility could process at most 28,000 packages per shift so 4,000 packages were left for the next shift. The data that was used to build the linear model did not reflect what happens when capacity is exceeded.

Competition for shared resources
: 

Shared resources are especially prone to introducing difficutl to model behaviors in systems. When multiple processes depend on the same resource, then the output of the system is affected by both the total demand for the shared resource as well as the timing of arrivals, priority rules, and interactin patterns between the various requests for processing. Contention for a shared resource can create a ripple effect in a system. A spike in demand in one process can consume the capacity of a shared resource and this blocks other independent processes tha depend on the same shared resource even. These kinds of contentions for shared resources exhibt what is known as path dependennce, that is the sequence of arrivals, in addition to the overall workload, affects the system output.

## Uncertainty and Risk Propagation

Much of the effort we put into data analysis and decsion support is focused on reducing uncertaintly. The last thing an executive decsion maker wants to hear in response to a question about how to proceed is “it depends.” In similar vein, decsion makers don’t want to be blindsided by unanticipated risks.

Compound uncertainties
: 

We can get use to have single point estimates when dealing with historical data. We can know for sure that last quarters revenue was $4.2 million dollars. We can’t know for sure that next quarters revenue will be $4.6 million. A more accurate description of what we can know is “We expect next quarter’s revenue to be between $3.9 million and $5.0 million with $4.6 million the most probable.” In addition to providing a false sense of certainty, single point estimate can hide important information lurking withing the distribution of possible values. For example, if being 20% under estimate has more negative impacts than being 20% over estimate, then decision makers need to know the probability of each. Single point estimates don’t tell us much about the tails of distributions. We might expect $4.6 million next quarter, but what is the probability of hitting only $3.6 million? Understanding the distribution can provide insight in the range of likely outcomes and not just focus on the single, most likely event.

The actual revenue next quarter will be influenced by multiple factors such as the nubmer of customers entering stores, the average order value, and the number of retruned items. Each of these factors is best described by a distribution rather than a single point estiamte. Monte Carlo simulations are especially helpful for understanding how the combinations of multiple uncertainties can affect an outcome of interest, such as the total quarterly revenue. (See Chapter 3 for more on Monte Carlo methods.)

![](assets/figure_2-3_point_estimate_vs_distribution.png)

Fat-tailed Distributions and Rare Events
: 

When we mention the word “distribution” you might think of a bell-shaped, normal curve. The normal distribution is assumed in many statistical tests and with good reason. Many distributions we encounnter in real life are normally distributed. One of the charactereistics of a normal distribution is that values tend to cluster around a central value. Another characteristics is that extreme values tapper off quickly. As we get farther away from the value of central tendency, the likelihood of an extreme value decrease percipitously. In fact, in normal distirbutions, the tails of the distribution decay exponentially. This means that events in these extreme ranges are vanishinly rare, think 1 in a billion chance.

Another class of distributions are refered to as fat-taile distributions. In these distributions, extreme events occur far more frequently. This is because fat-tailed distributions decay polynomally instead of exponentioally. Many real world phenomenon exhibit power law distributions, including wealth distribution, city populations, website traffic, and book sales. Billionares exist in increasing numbers, megacities natually occur from growth dynamics, a few websites dominate web traffic, and best selling books sell far more than most books.

The practical implication of the difference in tail behavior or normal and power law distributions is that extremely rare of virtually impossible events under a normal distribuiton is possible and occur with a knowable frequency under a power law distribution. The 2008 financial crisis is one example of a power law tail event that historical data analysis failed to predict.

When building simulations, we need to be particularly careful with choosing distributions. In power law distributions there are no typical events size around which others cluster. This means extreme values can strongly influence averages and mislead us about assumptions around central tendency. Standard deviations and variance can be infinite in some power law distributions so those statistics are useless for decision making. Conventional confidence intervals assume the variance of a distribution is finite and there is a central tendency, neither of which are true for some power law distributions. Although commonly used statistics are not helpful when dealing with power law distributions, simulations with appropriate power law distributions can help us better understand fat tail risks.

![](assets/figure_2-4_power_law_normal_comparison.png)

Figure 2.4 shows how the normal distribution forms a symmetric bell curve with thin tails that vanish by three standard deviations from the mean. Power law distributions, however, peak at small values and then decay slowly following the formula:

> P(X>x)∼x−α

The parameter, α (alpha), in a power law distribution determines how heavy the tail is. Power law distributions with with lower α values have heavier tails.

## Adaptive Behaviors

Adaptive behaviors in systems can be hard to predict. When we use use statistics to understand past behviors and apply those findings to expectations for the future, we implicitly assume the factors that drove past outcomes will continue to drive future outcomes in similar ways. This is a reasonable assumption in many cases. If a distributor has a 1 year history of recieving weekly orders for 1,000 to 1,100 units of a product from a retailer, then it reasonable to assume that pattern will continue for the next week. In cases like this, the actors in the system are behaving in a readily predictable way. The retailer and the distributor have shared interests and both benefit from predictable, collaborative behavior. This is not always the case.

In competitive environments, actors have incentive to adjust their own behavior in response to compentitors actions. Consider how traders behave in equity markets. High frequency traders take advantage differneces in stock prices on different exchanges. For example, a company announces better than expected earnings and this price of that stock on one exchange goes up while the price on a second exchange does not go up for fractions of a second. A high frequency trading algorithm detects the difference, buys shares on the slower exchange and sells them on the faster exchange. The time period when there is a difference in stock price across markets is known as an arbitrage window. Anytime there is an arbitrage window, actors have incentives to adapt their behavior to take advantage of it.

Customers adapt their behavior to changes in the quality or cost of a service. If a streaming company increases the cost of their premium service by $3 per month, some customers will absorb the cost, some will cancel, and some will down grade to a lower-tier service.There may be secondary effects, too, such as increased account sharing and shifting viewing time to competitor services. The change may also prompt streaming service competitors to offer special discounts to customers still paying for the premium service. The overall result of the price change may be an increase in reveneue in the short term but there is also a risk that over the long term customers will cancel or downgrade their service while customers that remain be less satisfied with the service.

## Path Dependence and Historical Contingency

Path dependecies describe how the state of a system are an outcome of present conditions as well as some sequence of events that occurred earlier in the history of the system. The historicial events may seem insignificant at the time but can lock a system into trajectories that last a long time. For example, in the 1970s two companies developed competing video cassette recording formats: Sony created Betamax and JVC created VHS. Betamax provided better picture quality and smaller cassettes. VHS offered longer recording time, 2 hours instead of 1 hour. In addition, JVC licensed VHS to competitors while Sony maintained stricter controls over Betamax. JVC’s approach lead to more manufacturers selling VHS players and that drove down costs. Lower costs drove higher adoption by consumers and higher adoption drove studios to prodouce more VHS formatted content, which incentivized even more adoption of the VHS format. This example shows how a single feature, video length, create an early adoption trend that is reinforced by other factors and leads to an insurmontable lead that competitors cannot overcome.

Other examples of path dependence occur when cities make choices about infrastructure. One city decides to develop a highway through downtown. This makes suburban areas more easily accessible prompting businesses to move stores and offices near highway exits in the suburbs where land costs and rents are less expensive. People move to the suburbs for larger houses and short commutes to work in the city. Over time, the ability of the city to generate tax revenue from businesses declines. Meanwhile, another city that choose to develop dense clusters around public transit stations with walkable neighborhoods finds its tax base grows over time as the city amenities and services drive up the value of the urban land. Each decision was made at a point in time but had lasting impact long into the future.

Traditional analytics, like any tool, work well for the uses it was designed for. When we vary from the assumptions underlying these tools, we run into problems. We have seen how feedback loops create dynamics that linear models miss and how delays can generate costs that are immediately obvious. Non-linear effects can cause phase transitions which are sudden and dramatic shifts in the state of a system while capacity constraints can invalidate our projections when we hit those capacity limits. Mulitple sources of uncertainty can compound difficulties in making predictions while adaptive behaviors undermine assumptions about the past being a good indicator of the future. Path dependencies can lock us into trajectories that are difficult and costly to undo. Fortunately for us, simulation models can help us understand systems subject to any of these factors.

# How to Identify Simulation Opportunities in Your Organization

By now it is clear there are use cases where traditional analytics are not a good fit, but how can we identify those? When should we consider using simulation models to address these use cases? To identify simulation opportunties in your organizaiton, you should:

*   Look for warning signs that traditional analytics will not be sufficient
    
*   Assess decision context characteristics and modeling feasibility
    
*   Build and evaluate the business case for simulation modeling
    

These steps will help us understand both the charcteristics of the problem and the importance of the problem to our organizaiton.

## Recognizing Simulation Modeling Opportunities

We can often find simulation modeling opportunties by considering use cases where traditiona analytics will be challenged and by looking for system characteristics that commonly appear in simulation use cases. Some examples of cases where traditional analytics will be challenged include:

*   If you are confronted with “what if” scenarios involving conditions that have never been observed historically, you have an obvious case where analytics base on historical data will not help.
    
*   Cases where feedback effects influence the outcome
    
*   Resource allocation problems with capacity constraints and queueing dynamics
    

Here are some examples of problems exhibiting classic simulation indicators: \* Persistent boom-bust cycles that seem irrational or unpredictable \* Unanticipated fragility in a system despite redundancy in the system \* Sudden drop in performance as the systems starts to approach capacty \* Unintended consequences from well-intentioned policy changes

Once you have candidate problem, the next step is to assess how well simulaton modeling can address the problem.

## Evaluating Simulation Fit

To determine if simulation modeling will prove useful, we need to consider the context in which decisons are made. Good candidates are problems which:

*   Involve high stakes choices where the wrong answer will incur high costs
    
*   The system operates over long time periods and delays and feedback matter
    
*   You are dealing with complex systems with multiple interacting stakeholders and competing objectives
    
*   There is no options to test or experiment in the real world
    

If we have a problem that is a good fit for simulation, we’ll want to make sure we can actually build and evaluate the model. For this, we’ll need:

*   A sufficient understanding of causal mechanisms in the system we are modeling
    
*   A commitment from stakeholders to engage with the process
    
*   Access to domain experts who can help with parameter estimation and validation
    

The last step in identifying simulation opportunities in your organization is building the busines case.

## Building the Business Case

Before stakeholder and experts will invest their time in a simulation modeling project, they’ll want to be sure it is worth the time and expense. We need to be able to create a business case for our projects. This starts with quantifying the value of the simulation and can include quantifying the cost of a poor decision versus the cost of building a simulation. Simulations can also help with stress testing strategies without changing production environments or running expensive experiments in the real world. If we are building simulations of complex business processes, we may find competitive advantages to optimizing those processes that were not apparent. Look for high impact projects such as initiatives where traditional analytics have failed, business needs where decisions have to made frequently and would justify the cost of simualtion modeling, and situations where stakeholders are frustrated with the current set of tools available to support their decsion making.

# Overview of the Simulation Technology Stack

To efficiently and reliably develop and deploy simulaiton models we need to leverage a sound technology stack that supports all phases of the simulaiton modelign lifecycle. In this book we take an opinionate approach that Python and AI tools coupled with a specification-driven development approach is the optimal way to build a wide variety of simulations. When it comes to deployment, we can take advantage of containers, cloud infrastructure, and data engineering workflows that have worked well for so many analytics and machine learning systems.

## Foundation: Python Numerical Computing Libraries

NumPy for numerical calculations
: 

NumPy is a numerical Python package for scientific and numerical computing in Python. It providing efficient implemention and support for large, multi-dimensional arrays and matrices, including an extensive collection of mathematical functions. NumPy includes the core data structure ndarray, which is an efficient array structure. One of its key featurs is that it enables applies a computation to an entire array at once, rather than looping through individual elements. This is known as a vectorized operation.

There are many advantages to using NumPy by data scientists building simulations. For data scientists building simulations, Vectorized operations are executed by compiled C-language code. This makes simulations and other computationally intensive models run orders of magnitude faster than if you had used pure Python. NumPys random number generation modules support multiple probability distributions widely used for simulation modeling.

SciPy for statistical distributions and optimization
: 

SciPy is a Python package that builds on NumPy adding modules that supports optimization, integration, interpolation, signal processing, linear algebra, and statistics. The stats module in SciPy provides a large set of functions related to probability distributions including beta, gamma, lognormal, and many of other distributions. In addition, SciPy has functions to fit distributions to empirical data; this is especially important when determining distribuitons from historical data. The integration module is helpful for system dynamics simulations that require integration while the interpolate module is useful for discrete simulations that require smooth function approximation.

## Domain-specific Simulation Frameworks

SimPy for discrete-event simulation
: 

SimPy, short for Simulation in Python, is a discrete event simulation (DES) framework that models systems where entities move through processes and make use of limited resources. SimPy is widely used for modeling operational systems with queues, bottlenecks, and resource constraints, such as manufacturing lines, hospital emergency departments, call centers, and supply chains. SimPy a higher level of abstraction for simulaiton modeling than NumPy or SciPy. SimPy promotes the use of Pythonic patterns to model complex systems. For example, it uses Python generators to define processes alignging with how we’d typically describe the system. Resources like servers, machines, staff and entities, such as customers, parts, patients, interact through events that occur in simulated time. SimPy is useful for simulating capacity planning challenges, detecting bottleneck, and performing what-if analysis.

Mesa for agent-based modeling
: 

Mesa is a Python framework for agent-based modeling (ABM) that enables you to simulate systems of autonomous agents interacting according to rules and creating emergent behavior from agent interactions. Mesa models individual entities, such as people, firms, vehicles, or cells. Each entity has unique attributes and decision-making logic operating in shared environments. A marketing team, for example, might model consumers as agents who make purchasing decsions based on their exposure to information about a product. The marketers can expose consumer agents to mass advertising as well as social media influencer content to determine whether traditional advertising or influencer marketing is more likely to promote adoption and reach a tipping point where word of mouth connections are sufficient to drive adoption.

Mesa helps data scientists with agent-based modeling by providing core ABM infrastructure that includes agent classes, spatial environments, such as grids, networks, and continuous space, and schedulers for activation order. It also has data collection tools. Mesa is especially usefule when working on problems where heterogeneity and local interactions drive system behavior. Applications like market dynamics, disease spread, traffic patterns, and organizational behaviors are all good candidates for using Mesa.

PyMoo for multi-objective optimization
: 

PyMoo is a Python framework for multi-objective optimization that enables you to find optimal solutions subject to competing objectives that can’t be simultaneously maximized. This is different from other packages described here which are used to build simulation processes. PyMoo is used when we have conflictig goals, such as minimizing costs while maximizing quantitiy or balancing risk vs return in a financial risk analysis. PyMoo generates the set of trade-off solutions where improving one objective requires sacrificing another (known as the Pareto frontier in optimization parlance). PyMoo implements several optimization algorithms lthan can handle complex constraint scenarios and large decision spaces efficiently. This is especially useful we run simulation model thousands of times with different parameters to discover optimal configurations. PyMoo can be combined with Mesa, SimPy, or custom simulations, to help identify solutions that balance multple business objectives.

## AI-Powered Development Layer

Large language models and large reasoning models have substantially altered the practice of software engineering so it should be no surprise that these tools are also essential componets of the simulation modeling tech stack. Domain experts in areas like supply chain management or financial risk analysis can describe their problems in natural language. From there, LLMs can help identify variables, distributions, and dependencies between system components. From there, domain experts can generate initial model implementaitons and rapidly prototype using Jupyter notebooks, which are widely used by data scientists.

AI tools like LLMs can help identify logical errors and bugs in generate code. They can also help with clarifying model assumptions and, when the model is fully designed, with optimizing code implementing the model. As simulation models produce results, LLMs can be used for analysis of system dynamics, feedback loop identification, as well as bottleneck detection.

Models are complex and details matter. Undisciplined vibe coding without a clear appreciation for the need for a structured development approach will lead to failure. In this book, we will use a specification driven development approach to defining simulation models. With this development practice, we define what a model should do rather than specify how to implement it. Model requirements are expressed in a combination of natural language for describing system components and their relationships and formal specifications which we we use for precise specificaitons, such as when specifying a mathematical formula. With well defined specifications, LLMs can translate specificaitons into executable simulation code. LLMs can be also be used to create unit tests and validation scenarios from model specifications. Another advantage of specification-driven development is that specifications can serve as documentaion as well.

## Production Deployment and Operations Infrastructure

Deploying simulations to production environments requires infrastructure that are reproducibile, scalable, and reliable. This combinations of non-functional requirements is not the result of any single component in the simulation tech stack, it is a by product of the way we combine and use several elements of the simulation tech stack. In particular, we will see throughout this book how we use the following to ensure a reliable and scalable production environment.

*   Docker containers package simulations components, including Python, libraries, and configuration files. Containers help guarantee consistent execution across development, testing, and production environments.
    
*   Cloud providers offer elastic compute resources that scale with variable simulation workloads. For example, you can run a Monte Carlo simulation requiring that requies 10,000 runs can distribute work across a large much less time than if we run the same simulation on a single server.
    
*   Orchestration tools help us manage complexity at scale. Kubernetes is designed to coordinate container deployment, scaling, and recovery of failed deployments. Workflow managers like Airflow manage complex simulation pipelines from data ingestion and preprocessing through model execution and result aggregation, to reporting.
    
*   Observability tools including metrics and log collection provide insights into the state of simulation runs.
    

Simulation modeling should leverage infrasturcture and tools already in place in your production environment. Our goal with simulation modeling is to expand the range of analysis we can peform, we are not trying to incrase the complexity of your computing infrastructure.

# Summary

Traditional analytics excels at describing the past but struggles when systems entail feedback, delays, non-linearities, and emergent behaviors. As data scientists, we need to expand our toolbox to include simulation modeling when we are dealing with these challenging dynamics. An expaned analytics technology stack makes simulation accessible through AI-assisted development and cloud deployments while integrating with existing workflows. In the next chapter, we will shift focus from the genearl simulation landscape to understanding how to analyze business problems with multiple uncertainties by using Monte Carlo simulations.

# About the Author

**Dan Sullivan** is an author, instructor, and architect specializing in data science, machine learning, and cloud computing. He is the author of several books about databases, cloud architecture, data engineering, and machine learning. His data science, data engineering, machine learning, and cloud architecture courses are available on O’Reilly, LinkedIn Learning, and Udemy. Dan has worked in multiple industries as well as academic research and holds a Ph.D. in bioinformatics and computational biology.