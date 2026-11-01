
In an agile environment, all that is needed is the right mentoring for implementing Kanban with best practices. To summarize, the role of a flow manager should target the following:

- Track work flowing in the system
- Remove blockers or risks
- Facilitate change and deliver in a timely manner
- Continuously improve and support the team

The request manager (in some teams it is also known as the *service request manager* [SRM]) is a role like the product owner role in the Scrum methodology. This role manages the flow of work within the team and drives discussions between different teams and stakeholders. The key expectation from this role is to improve customer interactions.

- Ordering work items in the flow
- Owning policies
- Ensuring governance
- Tracking risks

- Team
  - Cross-skilled members
  - Pull work from the workflow
  - Each team limited to 15 members

In the infrastructure world, the roles of SRM and SDM already exist, but they follow a rigid process. With a little mentoring on Kanban, these roles will become flexible and will adopt new ways of working. In fact, the ideal situation is that everyone in the team should become an SDM since the goals are to observe the work flowing in, pick up work, and ensure that it is resolved quickly. This is the end state where the team becomes self-driven and self-organized without the need for governance and flow management.

## Kanban Ceremonies

Similar to the Scrum method, Kanban teams also practice a few ceremonies that help them to understand the plan, look at their progress, mitigate risks, and prioritize stories as needed. See Table 5-2.

### Table 5-2. Kanban Ceremonies

| Ceremony | Frequency | Purpose |
|---|---|---|
| Iteration planning | Monthly (2 to 3 hours) | Review capacity, throughput, lead times |
| Story prioritization | Weekly (1 to 2 hours) | Revisit backlog and prioritize as per customer demands |
| Daily standups | Daily (15 mins) | Team connects on risks and plans for the day |

In addition to these ceremonies, teams that are building infrastructure as code have a ceremony for demonstrating their MVP to stakeholders. If the demo is approved by stakeholders, then the catalog item is moved to the production environment.

## Kanban Boards

The Kanban team visualizes its work through Kanban boards. These boards help teams look at the work flowing in their stream and decide on appropriate actions. These boards are like tables with defined columns wherein each column represents a state, like to-do, in-progress, deploying, etc. Each state represented on the board is called a *swim lane*, and it may have defined limits as well. For example, at any given time not more than four issues can be in the deploying state. This is known as *work-in-progress (WIP) limits*. These Kanban boards are an excellent medium to not only show project status but also put a limit and ensure visibility on the capacity of the team to deliver on the number of tasks/issues that are addressed in a specific timeframe. Figure 5-15 is an example representing a typical infrastructure operations team that manages operating systems. Each swim lane has a name and shows the issues underneath it. While these boards can be manually drawn, tools like JIRA help teams to visualize these boards better. As teams can view the board, they are encouraged to pull work and move them from one state to another. See Figure 5-15.

Windows/VMWare/VDI

### Kanban board

QUICK FILTERS: Only My Issues Recently Updated

![](img/f561fe6de202c361a03741b184bee8ea_101_0.png)

Figure 5-15. Kanban board sample

So, the Kanban methodology works on three key principles: visualizing work, limiting work, and iteratively working to improve with self-sustaining teams.

## Kanban Metrics

Please refer commonly used metrics for Kanban in Table 5-3 and Figure 5-16.

### Table 5-3. Kanban Metrics

| Metric | Description |
|---|---|
| Cycle time | Time when the customer submits a request until it gets resolved |
| Lead time | The actual time spent when the infrastructure IT team started working on that ticket until it gets resolved |
| Throughput | Number of work items completed per unit of time |

![](img/f561fe6de202c361a03741b184bee8ea_102_0.png)

Figure 5-16. Lead versus cycle time in Kanban

## Getting Started with Kanban

There are various tools that enable teams to practice Kanban. Atlassian JIRA is one such commonly used tool that provides templates for implementing the Kanban methodology. This tool can be installed and used as an on-premises solution or can be used through the cloud offering.

The tool has user-friendly templates to practice Kanban. Each project created in JIRA has the following specifications:

- Workflow
- Name
- Unique key/identifier

Every time a new project is created in JIRA, it prompts for the project template, the workflow to be implemented, and the project name that has a key that is used to identify tickets. See Figure 5-17.

![](img/f561fe6de202c361a03741b184bee8ea_103_0.png)

Figure 5-17. Creating a Kanban project in JIRA

Each project template is associated with a workflow that defines the journey of a ticket. As beginners, teams can leverage existing workflows, as shown in Figure 5-18, or create a custom workflow that addresses their project needs.

## Kanban software development

![](img/f561fe6de202c361a03741b184bee8ea_103_1.png)

Figure 5-18. Kanban workflow in JIRA

The workflow shown describes how a ticket will be placed in the backlog and then how it moves to a different state by the team members. The states mentioned in the workflow are Selected for Development, In Progress, and Done. These states are customizable and can be set in for different issue types like in this case Bug, Task, Sub-task, Story, and Epic.

Once the workflow is selected, the next step is to give a name and an identifier to the project. For example, if the project name is Custom-Kanban-Project, then the tool automatically generates an identifier that is a three-letter abbreviation. This identifier is something that gets tagged to each issue type. For example, stories written now in this project will have story numbers appended with the key. If this were modified with a value like OPS, then the story numbers would be generated as OPS-1, OPS-2, etc. See Figure 5-19.

## Kanban software development

![](img/f561fe6de202c361a03741b184bee8ea_104_0.png)

Figure 5-19. Naming Kanban projects in JIRA

It is important that a Kanban project creation is well thought through. If there is a need for customizing the states, issue types, identifiers etc., then this should be done during Kanban practices setup for the project.

Once the project is set up, the next important thing to set up is WIP limits. WIP limits are defined as the team’s capacity to address a minimum and maximum number of tasks at any given point in time. This helps teams to avoid overload across all the stages in the life of a story or an issue. Based on your current team size, skillset, and working hours, define the maximum limits for each of the swim lanes. Let’s study the process of how to calculate WIP limits.

## Steps for Defining WIP Limits

Let’s assume that we have created four swim lanes on the Kanban board named Development, Review, Testing, and Deployment. We will set the WIP for each of these four stages, which means finding the maximum number of issues or tickets that can be managed at each of these stages. Before we start with the process of calculating WIPs, first let’s divide each stage into two sections that will depict a value-added (VA) activity and non-value-added (NVA) activity.

A VA activity indicates actual work done, and an NVA activity refers to the wait or delay times that occur during work execution. For example, let’s divide the development stage into an NVA named “Ready for development” and a VA activity as “In development.” The state “Ready for development” indicates a list of prerequisites to be done before the actual development starts, and the state “In development” refers to the actual development state. So, each stage in the lifecycle will have VAs and NVAs, and this division will help us to estimate the right WIP. Divide each of the other stages as shown in Table 5-4.

### Table 5-4. Activities Categorization in Kanban Stages

| Development |  | Review |  | Testing |  | Deployment |  |
|---|---|---|---|---|---|---|---|
| Ready for development | **In development** | Ready for review | **Reviewing stage** | Ready for testing | **In testing** | Ready for deployment | **Deployment stage** |

VA activity

NVA activity

With this updated Kanban board, we will now proceed to estimate the WIP for all the VA states. The formula for calculating WIP for a Kanban board is as follows:

**WIP = Total Tasks * Time %**

*Prerequisite 1 is Total tasks = Team size / Efficiency => refers to the maximum number of tickets that can stay on the board. This formula needs the team size (for our example we will consider it to be five) and efficiency factor (which needs to be calculated, described as prerequisite 1).*

*Prerequisite 2 is Time % = [VA / Sum of VAs ] * 100 => refers to the percentage of time spent on VAs. To calculate this value, we need to find out the VA values for all the states (described as prerequisite 2).*
