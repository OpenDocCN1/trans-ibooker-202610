
Many processes work together to formulate a successful FinOps practice. Automation follows process.

![](img/4d4736bc293fd892d1da81802d0f9520_351_0.png)

Before you can build automation, you need to build out processes for that automation to follow.

It’s important to define who is responsible for (or “owns”) the process, what parts of the organization must follow the process, and to what goal or goals the process contributes. A clearly defined operating model outlines expectations for staff.

When you adopt third-party automation tools—covered in Chapter 21—they often dictate how the process must work. When using these tools, you may need to adjust your defined processes to suit.

Start by figuring out how items get added to the new process (onboarding), what is expected from teams (responsibility), how teams are informed about when the process should be followed and the outcomes of following the process (visibility/alerting), and finally begin following the process (action).

## Onboarding

Circumstances change, especially when you’re using the cloud. Your organization might add to its existing cloud deployments or go through a company merger. All processes should clearly define what changes with cloud growth; otherwise, you end up with a process that works for the present but not for the near future. Don’t just think about the now, think about what the future may bring, and start thinking ahead about which parts of the process will need to evolve so you’re setting yourself up for future changes and success. Getting buy-in from the teams that will be actioning the process should happen well before action needs to be taken, and setting expectations with them that there will be changes as the practice matures. Convey to teams the goal and outcomes that the new process is aiming to achieve.

### Stories from the Cloud—Mike

At Atlassian, there are specific processes for onboarding new cloud accounts. These processes define how an account is added to the existing billing structures and how FinOps tooling is added to the new account. Governance tooling is added to the account, enabling idle resource management and rightsizing recommendations. Any existing RIs in the account are merged into the centralized management by the FinOps team. Having this governance builds a standard approach to account management, removing the need for individual teams to manage their RIs, and allows for more savings over time. The items being governed will evolve as the FinOps practice matures, such as when we moved from using Trusted Advisor’s compute optimizations to using Compute Optimizer’s recommendations. This allowed us to cover more cloud services beyond the basic compute. The central team responsible for governance is constantly working on providing better tooling to work with the next order of magnitude of scale.

## Responsibility

Each process should be allocated to an owner. Effective processes clearly define who does what, and when, and how they do it. Do teams follow the process daily, weekly, monthly, or at some other frequency? Remember some processes will be followed by the FinOps practitioner (like buying commitments), while others are distributed (like rightsizing and usage optimization).

Responsibilities assigned to teams will grow as processes mature. Regularly communicating any increases in expectations will reduce the likelihood of teams claiming they were unaware of what they should have been doing and when.

Through each iteration of the FinOps lifecycle, you build on these processes. At the beginning of the cost visibility journey, for example, the only expectation of a team when they receive a cost report might be to read it. Later, teams might be asked to investigate and explain any costs over a certain threshold. Even later, teams might be required to revise budgets and forecasts when projections show goals won’t be met.

Once again, you’re gradually maturing FinOps. It’s impossible to have teams investigate costs during the first time through the FinOps lifecycle without first putting in place a well-developed cost allocation strategy and correctly forecasted budgets. Instead, you minimize time through the loop, because trying to build all the processes at once makes it impossible to track the impacts of each process.

![](img/4d4736bc293fd892d1da81802d0f9520_352_0.png)

In photography studio and theater lighting design, there’s a maxim that reminds the lighting technician to only move one light at a time—that way, they can see the impact of each change before making additional adjustments. The same holds true here: if you change processes related to your rightsizing or tagging and to your reserved instance or savings plan management at the same time, you may not be able to easily identify which change drove movement in your bill.

## Visibility

Both before and after an action is taken, you must build visibility. In the operate phase, processes that enable visibility ensure that everyone is aware of the changes required and those being made. The most common operational processes for visibility send automatically generated reports or alerts to teams to make them aware of events as they happen.

Reports should be clear and easy to understand using the FinOps common language we defined in Chapter 4. Training and documentation should be made available to ensure all staff are familiar with this lexicon. Alerting and anomaly detection should occur on predictions based on current metrics, allowing staff to respond before things are off track. Instead of waiting for a target line to be breached, forecast the near term to identify when action is required early. A good example of this is AWS Budgets, which allows alert configuration based on predicted end-of-month spend.

Over time, processes and communication channels will grow in importance and sophistication. The FinOps team can bring development teams, finance teams, and business teams to the table to learn what information can help them plan and make decisions more effectively, as well as to receive information about their plans. Over time, the amount and variety of information provided will increase, and it will be critical for FinOps practitioners to continuously improve how they communicate.

Many FinOps teams start by holding monthly or quarterly meetings with the technology teams building and running applications. The first meetings can be awkward, uncomfortable, and somewhat guarded, with neither side quite understanding what the other wants. Over time, as these discussions continue, the FinOps team works with the IT teams to advocate for them, to evangelize the good steps they are taking to understand and control costs, and to provide them with a better understanding of business goals and finance requirements. Records of these meetings show deeper conversations about complex cloud decisions the team is considering, and participants begin to come to the meeting prepared to talk about ideas to save costs, rather than being dragged through their own cost data.

Combining visibility with clear responsibility and role definition breeds trust and leads to partnership in action. Working with the various teams of your organization is one of the processes the FinOps team needs to build. Be careful not to overlook the processes related to operating a FinOps team successfully when building processes such as rightsizing and commitment-based discount program planning, which are more focused on the cloud providers.

## Action

The action stage is where something actually happens—processes involving activities that enable your organization to reach its FinOps goals, from generating new reports to turning actual cloud resources on and off. Action processes are often good candidates for automation. When we discussed rightsizing in Chapter 15, we highlighted that recommendations are often generated by the centralized FinOps team and then distributed to engineering teams so they can investigate and adjust their resources where appropriate. Without clear processes for how rightsizing recommendations are
