
- Are these storage solutions on-premise? In the cloud? Both?
- Who has access to these systems and each data domain within them?
- How are permissions granted per system, and who grants them?
- Who oversees security for these systems?

Data Quality:

- Does your organization produce unique data? If so, which colleagues manage it?
- Who cross-checks this data for quality control?
- How long does the full data production process take, from collection to dissemination?
- How (if at all) does your organization share data across departments?
- If your organization publishes data that turns out to be incorrect or non-compliant, what’s your process for correction?
- Does this process apply to all aspects of data collection/sharing, or does it change per dataset or data domain?
- Which format(s) do you store data in? Does this format ever change? If so, when?

Since you’re writing down answers to these questions, this is a good time to remind you how crucial documentation is for good data governance. Keep the documentation you’re currently writing in an accessible space that all data stewards will use and access. When considering which documentation tool to use, start by asking which products your organization already uses.

For instance, if your technical team uses Jira to manage sprints, you can use Confluence for data governance documentation. The same parent company (Atlassian) makes both tools, which means that Jira and Confluence integrate with each other. This works in your favor by helping your data analysts, scientists, and engineers map their current tasks to your data stewards’ work.

Let’s say you have an epic in Jira that captures all the child issues needed to design a data model. Those child issues might include tasks like writing acceptance criteria, building Node diagrams, implementing the data model, and migrating data to your database server. You can link this epic and respective child issues in Jira to your data governance documentation in Confluence. This will give the data architects and engineers who design data models a guide to have on hand as they work.

Ideally, some of these data architects and engineers serve as stewards on your data governance council and helped write the documentation for conducting quality assessments. Using the right integrated tools gives them an easy way to link technical tasks with your data governance plan, allowing them to see how their work affects the big picture.

Thus far, you've assessed and documented how your current data governance stacks up against compliance, quality, and architecture. Now that it's on paper, the next step is to build a process map which illustrates the knowledge you've gathered.

## Create a Current State Process Map

Your process map will visualize all aspects of the process you're trying to fix through your data-focused project. Done well, it's a collaborative effort amongst data stewards that breaks down silos between teams.

Breakdowns in data-sharing result from a lack of trust and transparency. It's one reason why you should have a single data governance council, rather than creating separate councils per team. When you document your current data state holistically, then share it with all members of your data governance council, you ensure that everyone has the same visibility. This will help your stewardship team write a process map that's accurate and holistic.

As your data stewardship team builds the current state process map, keep some crucial ground rules in mind. You'll note that all the work we've done thus far, from crafting your data's mission statement to building a process map, is cultural. If you don't know why and how you want your business to use data, any data-focused projects will fail to make their biggest possible impact.

On a similar note, if your colleagues worry they'll get punished for telling the truth, they won't be honest about current problems or contribute to solutions. To encourage psychological safety, the University of New Hampshire suggests using these ground rules throughout the process:²

- Keep an open mind to change and maintain a positive attitude.
- Leave rank at the door; one person = one voice, regardless of position.
- Everyone on the team participates.
- Stay focused.
- Don't leave in silent disagreement.
- Practice mutual respect - do not interrupt; there are no dumb questions or ideas.
- Offer honesty.
- No blame - this is a blameless environment.

2. https://www.unh.edu/lean/phase-3-develop-current-state-process-map

Once you have set and shared those ground rules, this process flow map is an ideal task for your data stewards to co-own. Depending on how your organization uses data, and how many data projects your organization has in progress, your data stewards might need to make several process maps. In that case, you can divide the process maps amongst your data stewards according to their expertise and involvement with these projects.

Use the following template to create a process map for your data governance council's project, which you picked in Chapter 3. Build Your Data Governance Council. on page 27. Choose one specific process that you're trying to improve and add every step it takes to currently complete that process, no matter how small. Your goal is to show all the work needed to complete one specific process. Your end result will be the blueprint for the step that you'll take next.

![](img/22314b4da4bd5ef2d477a86ed13bbd6c_61_0.png)

As you document each step per process, be honest above all else. While each process will have exceptions and/or desired flows, it's essential to make process maps which accurately show what really occurs. If this is nerve-racking, I understand. Even when organizations say they want to use data more effectively, they won't always do the tough, introspective work required.

If you're not cringing at your current process map, there's a good chance it isn't honest enough. And without the psychological safety established earlier, your stewards won't feel comfortable telling the truth. That derails all your efforts in the long run.

As with all templates in this book, use this process map to guide you while making one that’s unique to your organization. From here, you can get more granular about which parts of the process each department owns. This is an awesome opportunity to find possible roadblocks and start solutioning for your future state. That’s what you will make next.

## Design Your Desired Future State for Data Governance

Once you’ve confirmed your data’s current state, next comes the fun part: defining what you want your data’s future state to look like.

When you’ve finished your process map(s), you’ll have a baseline to make effective changes from. This will give your data stewards the chance to find opportunities for process improvement. If creativity strikes while you and your stewards build your process maps, capture those ideas in writing. Using a shared tool like Confluence works best because it lets you write down your thoughts and share them with the broader governance council. These ideas can form the basis of your data governance backlog, which allows your team to review data issues regularly.

For example, let’s say you’re the CTO of a nonprofit that distributes state government aid for several social services, from preschool to emergency housing. Building your process maps revealed that your nonprofit stores the same client data on several disparate servers. Most of your clients use more than one social service, yet none of your data servers integrate with each other. As a result, your organization stores client data on several servers and your social workers have no visibility into how many programs each client uses. This might cause your social workers to suggest services their clients already use, or lose track of which clients no longer need help.

In this case, you’ll want to design a future state that aligns your data management against data architecture, quality, and compliance. Start by asking yourself some questions under each pillar:

Data Architecture:

- Can you consolidate data into fewer locations rather than storing it in several server/cloud environments?
- Do you rely on Excel formulas for much of your data management? If so, which tools might help you automate some of that work?
- What’s a realistic timeline for moving data out of one system into another? Do you use software or database vendors that include data migration in their services?
