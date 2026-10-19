
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

* Adding customer service agents to assist customers to understand the impact on gross sales revenues
* Extending open hours of stores by varying amounts to find the optimal number of hours to be open
* Delayed delivery of products to understand the impact on store revenue
* Combining multiple changes, like increasing the number of customer assistants and extending hours

Simulations are particularly useful when we want to reason about novel situations. Statistical and machine learning models are trained on historical data, so they are most reliable when the future resembles the past. But strategic decisions, like deciding to enter a new market, restructure a supply chain, or change pricing are situations where the future will definitely differ from the past. Simulation models the mechanism of change rather than identifying a pattern, allowing us to explore conditions beyond what is captured in historical data.

# Case Study: Predicting Financial Contagion in Banking Networks

In 2008 the global economy experienced a sudden downturn triggered by a collapse in the United States housing market. A build up in housing values coupled with risky subprime mortgages created a market bubble. When the bubble collapsed, housing values fell and many homeowners defaulted on loans. The impact of those defaults was not limited to the banks that made the original loans, however. This is because financial institutions bundled groups of mortgages into securities that were sold to investors. This spread the risk of default to a much broader set of institutions and investors.

The value of these securities fell as defaults increased. This was a problem for banks and other institutions that held these securities and had to comply with an accounting method known as marking to market. Basically, they had to report the value of their assets, including mortgage backed securities, at their current market value and not the original value when the asset was acquired. In 2008, this resulted in many banks reporting losses and insufficient capital to meet regulatory requirements. As a result, these banks faced additonal regulatory restrictions, interventons by regulators, and in some cases a loss in confidence that could lead to a bank run.

Bank crisises occurred prior to 2008 and regulators had tools for assessing the resilience of financial institutions but those tools did not identify the lack of resilience in the financial system in 2008. A study by Prasanna Gai and Sujit Kapadia for the Bank of England in 2010 used simulation modeling to understand the components and interactions of the financial system that led to a state of low resilience and risk of financial contagion. Gai and Kapadia’s study is described in [“Contagion in financial networks”](https://www.financialresearch.gov/working-papers/files/OFRwp-2015-21_Contagion-in-Financial-Networks.pdf).

The simulation is based on complex network theory. It models banks and relationships between banks in terms of claims and obligations. For example, if Bank A lends money to Bank B, then Bank A has a claim on Bank B and Bank B has an obligation to Bank A. The links between banks represent claims and obligations and those links are weighted according to the based on the value of the claim or obligation.
