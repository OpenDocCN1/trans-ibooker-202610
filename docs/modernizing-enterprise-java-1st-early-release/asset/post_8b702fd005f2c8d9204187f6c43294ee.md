
approaches or even standards that followed them. We usually also imply that those systems would need a replacement, which ultimately contributes to the perceived negative connotation. Thankfully, this isn’t always true. Legacy also is a beautiful word to describe achievements and heritage. Calling something “legacy” doesn’t automatically make it outdated and unusable. There are plenty of reasons to keep the legacy systems in place, including:

- The system works as designed, and there is no need to change.
- Business processes implemented are no longer known or documented, and replacing them is expensive.
- The cost for replacing a system is higher than the benefit of keeping it unchanged.

The book “Working Effectively with Legacy Code” by Michael Feathers provides programmers with techniques to cost-effectively handle common legacy code problems without having to go through the hugely expensive task of rewriting all existing code.

Michael Feathers said: “To me, legacy code is simply code without tests.” If I read the term “legacy” today, it primarily refers to monolithic applications. There are various approaches to handle legacy applications in a modern enterprise landscape, and picking the right one is the first and most crucial part of the modernization journey.

We’ve only talked about individual systems so far. And developers usually only care about this specific system scope. Modernization plans should follow overarching company goals and should also take the company-wide IT strategy into account. A particular exciting approach for cloud migration is presented in Gregor Hohpe’s book “Cloud Strategy - A Decision-based Approach to Successful Cloud Migration”. It is a must-read if you want to know more about building the abstraction above individual migration efforts.

## Managing Legacy

Every successful journey begins with a first step. The first step for an application migration journey is the assessment of the existing applications. We assume that you know the company-wide goals and directives. We can map them into assessment categories now. Another source for assessment categories is technical requirements—for example, existing blueprints or recommended master solutions or framework versions. Building and updating this list of assessment categories should be a recurring task that becomes part of your governance process. Ultimately you can derive migration criteria from these assessment criteria and use them as decision-making cornerstones for your modernization journey.

## Assessing Applications for Migration

When assessing a migration or modernization effort, it is essential to consider the specific challenges that motivate or influence your organization. Some examples of challenges that organizations might face:

### Limited budgets for development

Development teams need to become more efficient, and their velocity has to increase. Instead of working with complex specifications, they aim to switch to lightweight frameworks and pre-built functionalities. Modernizations usually should be scheduled as part of an ongoing development or maintenance project.

### Lack of in-house skills

The team skills for existing in-house technologies are decreasing. Examples for this can be host programming or even earlier versions of Enterprise Java specifications that are no longer taught or state of the art. Changing existing systems that use older technologies might need to add specific skills for the development project.

### Perceived risks

Following a famous proverb popularized around 1977: “if it ain’t broken, don’t fix it”, we do see a lot of perceived risks to change well-established and running software. The reasons for this can be plentiful and range from knowledge issues about the system to fear of stopped production in factories. These risks need to be addressed individually and mitigated through suitable actions in the migration plan.

### No known predictable process

This book helps you with this particular point. Navigating the unknown can be a considerable challenge. Having a proven and repeatable process for modernization efforts in place that all parties respect and follow is critical for success.

### Real effort estimation

Estimating migration efforts should not be magic. Unfortunately, many companies have a minimal idea about the genuine efforts to modernize Enterprise Java applications. Following a predictable and optimized approach will remove this challenge.

Turning the above challenges into actionable items for your assessment can look like this:

- Predicting the level of effort and cost
- Scheduling application migrations and handling conflicts
- Identifying all potential risks at a code, infrastructure, process, or knowledge level
- Predicting the return on investment to make the business case
- Identifying and mitigating risks to the business
- Minimizing disruption to existing business operations

It is sufficient to do this in a spreadsheet or document if you are only looking at a single application. However, every mid to large-scale effort needs a better solution. Large-scale efforts need automated routines and rules to assess an install base and link applications to business services to plan the next steps reliably. An open-source and straightforward way of gathering and managing all relevant information comes from the Konveyor project. It combines a set of tools that aim at helping with modernization and migration onto Kubernetes.

The Konveyor subproject Forklift provides the ability to migrate virtual machines to KubeVirt with minimal downtime. The subproject Crane concentrates on migrating applications between Kubernetes clusters. Also part of the suite is Move2Kube to help accelerate the re-platforming of Swarm and Cloud Foundry-based applications to Kubernetes.

For application modernization, in particular, Konveyor offers the Tackle project. It assesses and analyzes applications for refactoring into containers and provides a standard inventory.

### Tackle Application Inventory

Allows users to maintain their portfolio of applications, link them to the business services they support, and define their interdependencies. The Application Inventory uses an extensible tagging model to add metadata which is a great way to link migration categories, as discussed earlier. The Application Inventory is used to select an application for an assessment by Pathfinder.

### Tackle Pathfinder

It is an interactive, questionnaire-based tool that assesses the suitability of applications for modernization so that they can be deployed in containers on an enterprise Kubernetes platform. Pathfinder generates reports about an application’s suitability for Kubernetes, including the associated risk, and creates an adoption plan. Pathfinder does this based on the information present in the application inventory and additional assessment questions. If a fictitious application depends on a direct host system connection, it might disqualify this particular application for a migration to Kubernetes because it would overload the host parts. Some examples of assessment questions are:

- Are third-party vendor components supported in containers?
- Is the application under active development?
- Does the application have any legal requirements (e.g. PCI, HIPAA)?
- Does the application provide metrics?

We strongly recommend looking at Pathfinder to manage large-scale modernization projects across complete landscapes. It will help you categorize and prioritize applications in your scope today and continuously track your migration assessment for future changes.

### Tackle Controls

Controls are a collection of entities that add different values to the Application Inventory and the Pathfinder assessment. They comprise business services, stakeholders, stakeholder groups, job functions, tag types, and tags. In addition, you can capture company or project-specific attributes by implementing your own entities. This will filter your application inventory by for example all applications used by a certain “job function”. E.g. to identify all applications used by the human resources department.

### Tackle DiVA

Finally, DiVA is a data-centric application analysis tool. As a successor to the project WindUp, it is the most exciting project to look at if you want to assess individual applications. It focuses on the traditional monolithic application and currently supports Servlets and Spring Boot applications. You can import a set of application source files (Java/XML) and DiVA then provides the following:

- Service entry (exported API) inventory
- Database inventory
- Transaction inventory
- Code-to-Database dependencies (call graphs)
