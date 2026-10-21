
- Standardized methods enable data ingest, transformation, validation, and preparation.
- Data governance expedites value and spans compliance, risk, and regulation related to data (including privacy, security, and access controls).
- Responsible data policies specify access rights, “right to see” authorizations, ethical principles, and acceptable applications for data usage across the organization.

## Data Strategy

Moving from piloting to operationalizing AI requires a strategy to help align data capabilities with AI needs. This is your first step to achieving a scalable and repeatable AI model development process and operationalizing AI.

A data strategy is essential to realizing AI goals and scaling value creation through the following areas of focus:

- Identifying AI data needs and gaps to include the practical or legal constraints to usage of data
- Identifying ways to acquire/collect data to fill gaps and identify nontraditional data sources for added insights and integrating this data into existing data environments
- Creating a centralized vision to align data capabilities with needs of AI projects
- Aligning data management, transformation, quality, and governance functions toward building up and scaling AI capabilities
- Enabling a consistent view of data-related capabilities so they can evolve with AI needs
- Defining concrete actions, trade-offs, and metrics to help assess and prepare data capabilities
- Tracking data lineage, provenance, and quality to aid AI transparency and explainability

Your data strategy needs to answer the following questions in the AI context:

- What challenges might AI solve for your organization where traditional analytics has fallen short?
- How will the strategy put the organization on a path to realizing the goals?
- What concrete steps can help implement the strategy?
- What actual impacts can be tied to the strategy?
- How will aligning resources with the strategy support the mission and scale value creation?

While the path data takes between phases will differ for every organization, the data strategy development approach in Table 4-1 organizes data strategy development and implementation into four phases that are useful as a template for most organizations.

## Table 4-1. Suggested data strategy development phases

| Phase | Actions |
|-------|---------|
| Identify | - Identify data your organization creates, collects, or is otherwise able to access to train AI models and scale AI needs.<br>- Identify how data moves through the enterprise and how it aligns with AI data needs.<br>- Identify gaps in data availability and potential ways to acquire/collect data.<br>- Identify nontraditional data sources, which can offer indirect insights. |
| Assess | Assess your current data capabilities and evaluate data gaps:<br>- Evaluate data gaps, sources to fill the gaps, and alternatives in cases where there is availability or practical and/or legal constraints to acquiring or using the data.<br>- Conduct scorecard assessment of your current data capabilities against key drivers of AI data readiness: availability, accessibility, timeliness, accuracy, usability, form, quality, provenance, and security.<br>- Leverage assessment criteria in the scorecard to measure how the organization stacks up on people, processes, tools and technology, and culture needed. |
| Align | Define the data strategy that will help your organization address AI needs and get the most value out of data:<br>- Review results of scorecard assessment to determine your organization’s priority needs.<br>- Define data principles on which the organization agrees to operate for data use and management.<br>- Align these with analytics/AI opportunities to prioritize goals and define subsequent objectives. |
| Implement | Implement data strategy and supporting plans to help you get the most value out of your data:<br>- Develop an implementation plan, allocate resources, define KPIs, and develop a change management plan.<br>- Execute on data strategy-driven initiatives, regularly evaluating outcomes. |

As part of strategy development, AI decision makers should work to ensure active participation from data owners, business stakeholders, chief information officers, and chief data officers. This will help the strategy reflect a holistic view regarding data and its applicable use cases within your organization.

## DataOps: Operationalizing Your Data Strategy

DataOps is the methodology to operationalize your data strategy to deliver data at scale for enterprise AI needs. Gartner defines DataOps as a collaborative data management practice focused on improving the communication, integration, and automation of data flows between data managers and data consumers across an organization.⁷

Supporting AI data needs at scale requires senior management focus. Leaders can ensure successful strategy execution and prevent data “siloing” within individual business units. Table 4-2 lists some of the key players and teams you should include in your organization as you begin to build your data strategy.

## Table 4-2. Key players in DataOps

| Key player | DataOps role |
|---|---|
| Chief data officer | The chief data officer’s role is part data strategist and adviser, part steward for improving data quality, part evangelist for data sharing, part technologist, and part developer of new data products.ᵃ |
| Enterprise information management (EIM) group | Develop mechanisms that communicate, share, and reinforce the value of data across the organization. Establish policies and procedures of governing and managing data assets using repeatable processes and frameworks. Ensure data is treated as a strategic asset. |
| Cross-functional teams | DataOps practitioners who enable a culture of cross-functional ownership, repeatable processes, and continuous delivery of data capabilities with a variety of industry-leading technologies and tools. |
| Business unit leads | Leadership or designated representative of operating units or business functions who create/source and manage their data. |

> a “Open Data Policy—Managing Information as an Asset”, Project Open Data, accessed July 20, 2021.

The chief data officer provides an anchor point for alignment. This person is also responsible for chartering enterprise information management groups to standardize and scale data management across the organization.

Together, these groups develop mechanisms to communicate, share, and reinforce data’s value across the organization. The goal is to change staff behavior so data is treated as a strategic asset. These data advocates work together with cross-functional DataOps teams and business unit leads to serve as essential catalysts for achieving a data-driven organization.

There are several ways leadership and key players can support the data strategy:

- Integrate data sources in proportion with business and mission goals.
- Make re-creatable data environments to support AI transparency and explainability.
- Monitor DataOps health to provide valuable direction and adjust operations.

Data tools and pipelines support your organization’s AI growth and related capabilities. The following principles offer practical ways to keep up with evolving needs:

### Target an evolutionary data architecture

Data architectures must be designed with flexibility and adaptability in mind to evolve at the rapid pace of innovation. Maintaining the proper design abstraction allows for the component replaceability necessary to keep up with AI’s rapid evolution.

### Architect data pipelines for scale and flexibility

Similar to the overall data architecture, designing data pipelines for scale and flexibility at the beginning of your project is far more efficient than attempting to scale at subsequent stages of growth.

### Institute API interfaces as the building blocks for data sharing

An application programming interface (API) defines how to push or pull data during a data exchange. APIs help bridge data silos and break them down into more usable parts.

## Data Preparation Activities

Taking your data strategy from concept to reality requires data preparation activities to feed the appropriate information into your AI application. This section outlines important considerations and shares practical tips for data ingest, transformation, and validation.

## Ingest

Ingest enables raw data from multiple sources to be combined into a single source for an AI model’s data pipeline. Data ingest can support structured
