
- Engineers spend the majority of their time making forward progress on core business goals.
- Bug fixing and other reactive work takes up a minority of the team’s time.
- Engineers spend very little time disoriented or trying to find where in the codebase they need to plumb through changes.

## If your team is doing poorly

- Engineering time is wasted rebuilding things when their scaling limits are reached or edge cases are hit.
- Teams are distracted by fixing the wrong thing or picking the wrong way to fix something.
- Engineers frequently experience uncontrollable ripple effects from a localized change.
- People are afraid to make changes to the code, aka the “haunted graveyard” effect.³

## How observability is related

Observability enables teams to understand the end-to-end performance of their systems and debug failures and slowness without wasting time. Troubleshooters can find the right breadcrumbs when exploring an unknown part of their system. Tracing behavior becomes easily possible. Engineers can identify the right part of the system to optimize rather than taking random guesses of where to look, and can change code when attempting to find performance bottlenecks.

## Release on a Predictable Cadence

The value of software development reaches users only after new features and optimizations are released. The process begins when a developer commits a change set to the repository, includes testing and validation and delivery, and ends when the release is deemed sufficiently stable and mature to move on. Many people think of continuous integration and deployment as the nirvana end-stage of releasing. But CI/CD tools and processes are just the basic building blocks needed to develop a robust release cycle. Every business needs a predictable, stable, frequent *release cadence* to meet customer demands and remain competitive in their market.⁴

## If your team is doing well

- The release cadence matches business needs and customer expectations.
- Code gets into production shortly after being written. Engineers can trigger deployment of their own code after it’s been peer reviewed, satisfies controls, and is checked in.
- Code paths can be enabled or disabled instantly, without needing a deployment.
- Deployments and rollbacks are fast.

## If your team is doing poorly

- Releases are infrequent and require lots of human intervention.
- Lots of changes are shipped at once.
- Releases have to happen in a particular order.
- The sales team has to gate promises on a particular release train.
- Teams avoid deploying on certain days or times of year. They are hesitant because poorly managed release cycles have frequently interfered with quality of life during nonbusiness hours.

## How observability is related

Observability is how you understand the build pipeline as well as production. It shows you any performance degradation in tests or errors during the build-and-release process. Instrumentation is how you know whether the build is good, whether the feature you added is doing what you expected it to, and whether anything else looks weird; instrumentation lets you gather the context you need to reproduce any error.

Observability and instrumentation are also how you gain confidence in your release. If it’s properly instrumented, you should be able to break down by old and new build ID and examine them side by side. You can validate consistent and smooth production performance between deployments, or you can see whether your new code is having its intended impact and whether anything else looks suspicious. You can also drill down into specific events—for example, to see which dimensions or values a spike of errors all have in common.

## Understand User Behavior

Product managers, product engineers, and systems engineers all need to understand the impact that their software has on users. It’s how we reach product-market fit as well as how we feel purpose and impact as engineers. When users have a bad experience with a product, it’s important to understand what they were trying to do as well as the outcome.

## If your team is doing well

- Instrumentation is easy to add and augment.
- Developers have easy access to key performance indicators (KPIs) for customer outcomes and system utilization/cost, and can visualize them side by side.
- Feature flagging or similar makes it possible to iterate rapidly with a small subset of users before fully launching.
- Product managers can get a useful view of customer feedback and behavior.
- Product-market fit is easier to achieve.

## If your team is doing poorly

- Product managers don’t have enough data to make good decisions about what to build next.
- Developers feel that their work doesn’t have impact.
- Product features grow to excessive scope, are designed by committee, or don’t receive customer feedback until late in the cycle.
- Product-market fit is not achieved.

## How observability is related

Effective product management requires access to relevant data. Observability is about generating the necessary data, encouraging teams to ask open-ended questions, and enabling them to iterate. With the level of visibility offered by event-driven data analysis and the predictable cadence of releases both enabled by observability, product managers can investigate and iterate on feature direction with a true understanding of how well their changes are meeting business goals.

## Using the OMM for Your Organization

The OMM can be a useful tool for reviewing your organization’s capabilities when it comes to utilizing observability effectively. The model provides a starting point for measuring where your team capabilities are lacking and where they excel. When creating a plan for your organization to adopt and spread a culture of observability, it is useful to prioritize capabilities that most directly impact the bottom line for your business and improve your performance.

It’s important to remember that creating a mature observability practice is not a linear progression and that these capabilities do not exist in a vacuum. Observability is entwined in each capability, and improvements in one capability can sometimes contribute to results in others. The way that process unfolds is unique to the needs of each organization, and where to start depends on your current areas of expertise.

*Wardley mapping* is a technique that can help you figure out how these capabilities relate in priority and interdependency with respect to your current organizational abilities. Understanding which capabilities are most critical to your business can help prioritize and unblock the steps necessary to further your observability adoption journey.

As you review and prioritize each capability, you should identify clear owners responsible for driving this change within your teams. Review those initiatives with those owners and ensure that you develop clear outcome-oriented measures relevant to the needs of your particular organization. It’s difficult to make progress unless you have clear ownership, accountability, and sponsorship in terms of financing and time. Without executive sponsorship, a team may be able to make small incremental improvements within its own silo. However, it is impossible to reach a mature high-performing state if the entire organization is unable to demonstrate these capabilities and instead relies on a few key individuals, no matter how advanced and talented those individuals may be.

## Conclusion

The Observability Maturity Model provides a starting point against which your organization can measure its desired outcomes and create its own customized adoption path. The key capabilities driving high-performing teams that have matured their observability practice are measured along these axes:

- How they respond to system failure with resilience
- How easily they can deliver high-quality code
- How well they manage complexity and technical debt
- How predictable their software release cadence is
- How well they can understand user behavior
