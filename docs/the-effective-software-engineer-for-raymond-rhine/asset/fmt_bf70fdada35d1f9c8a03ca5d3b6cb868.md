
Recall the section [“Outcomes Versus Outputs: Solve the Right Problem”](#ch01.html#ch01_outcomes_versus_outputs_solve_the_right_problem_1768431688681838) from [Chapter 1](#ch01.html#ch01_the_foundations_of_effectiveness_1768431688681625). Always ask: “If we do this, what is the outcome and is it significant?” For example:
* Shipping a minor UI tweak that affects 0.1% of users versus tackling a performance issue affecting every page load. The latter likely has a higher impact.
* Automating a process that saves engineers five minutes a day versus building a feature that could attract new customers. The latter likely aligns more with business growth.

This doesn’t mean ignoring all small tasks (some low-hanging fruit can be quick wins). But have a sense of proportion and priority.

### Ask why.

When given a project, ask “What goal does this serve?” If it’s not clearly tied to a goal, dig deeper. Maybe the requirement is outdated or mis-prioritized. You might discover a more impactful problem to solve instead. For example, a PM might ask for an analytics report feature. By asking why, you find it’s to understand user drop-off. Perhaps an alternative solution is to improve the drop-off rate instead of just reporting on it. That might be more effective.

### Use an effort versus impact matrix.

A common strategic tool is to list potential tasks and estimate their impact (e.g., low/medium/high effect on revenue, user satisfaction, or efficiency) and the effort required to complete them (low/med/high time cost). Try to do the high-impact, low-effort items first (quick wins). High-impact, high-effort ones might become projects to plan. Low-impact, high-effort tasks likely should be dropped or deprioritized (those are classic busy-work candidates).

### Be willing to challenge assignments.

This can be touchy, but if you suspect a project is a lot of work for little gain, politely raise the concern. Use data if possible: “This feature is projected to be used by less than 2% of users—meanwhile, 30% of users are complaining about load times. Perhaps we shift some focus to performance, which could benefit all users.” Managers/PMs might resist, but a good org will listen if you present a case well. Even if they don’t pivot now, they’ll consider it in future planning. This shows you’re thinking about ROI, not just blindly coding, which marks you as a strategic asset.

### Stay informed about business priorities.

Read the product strategy docs, attend product review meetings if invited, and understand how the company makes money or what usage metrics matter. Then align your suggestions and work with those. For instance, if the business is focusing on user retention this quarter, think about what engineering work could boost retention (maybe reliability improvements, faster load times, or adding a feature that brings users back). Propose those or prioritize them in your own task list. Work on things the company cares about and you’ll multiply your impact (and your work is more likely to get noticed).

### Consider the 80/20 rule.

The 80/20 rule, also known as the [Pareto principle](https://en.wikipedia.org/wiki/Pareto_principle), states that for many outcomes, roughly 80% of consequences come from 20% of the causes. Identify what those key features or components are in your product. Ensure those are robust and evolving well. Spending days perfecting an admin panel that almost no one uses instead of enhancing the core feature everyone uses is misallocation. Strategic thinking is recognizing where the leverage is.

Next, let’s discuss what it means to align your work more explicitly with the goals of the business you’re working at.

# Aligning Engineering with Business Goals

To be truly effective, engineering effort must serve the broader company mission. That means understanding business goals and translating them into engineering plans. There are a few ways to ensure your efforts are aligned with the overall goals of your organization:

### Choose projects with strategic value.

As a senior IC, you often have some choice in what to work on (or how to approach a goal). Tilt toward efforts that align with company strategy:
* If the strategy is expanding to new markets, perhaps focus on making the product easily localizable or scalable internationally.
* If cost reduction is a major goal, maybe that optimization project that saves on cloud costs should be prioritized over a slightly nicer UI feature.
* If data privacy is now crucial due to regulations, focusing on compliance engineering has high business importance, even if it’s not “cool” technically.

This doesn’t mean dropping all other work, but keeping an eye on “what does the company need most from engineering now?” and contributing there. It will maximize your perceived value as well as actual company success.

### Be solution-oriented to business problems.

Sometimes the business will express a problem without a clear engineering ask. Effective senior ICs will try to solve it, even if it’s not a purely technical problem. Consider this example: customer support says many users complain they don’t know how to use feature X (a business problem: low feature adoption). Perhaps the solution is an in-app tutorial (so a bit of design, a bit of engineering) or adjusting the UX flow. Bringing a proposal to tackle that user problem even though it didn’t come as an engineering ticket shows strategic initiative. It’s bridging the gap between business need and tech execution—which is what staff engineers are often expected to do.

### Establish guardrails.

Sometimes alignment means pushing back on something that will technically undermine long-term business ability. For example: product wants to rush a feature that hacks something insecure. A strategic IC communicates the risk: “If we launch this way, we risk a security breach that could cost us users and compliance fines. How about we take two extra weeks to do it safely?” You’re aligning with the fundamental business goal of preserving trust and avoiding legal trouble, even if the immediate feature goal says, “Launch ASAP.” This is thinking one step ahead about consequences—a very strategic trait.

### Speak business language.

When communicating with non-engineers, frame your ideas by how they support business objectives. [Chapter 4](#ch04.html#ch04_collaboration_and_cross_functional_influence_1768431688998639) mentioned this in the context of influencing PMs. Company leadership especially responds well when outcomes are included: revenue, cost, user growth, risk mitigation. So if you’re advocating for technical work (like rearchitecting a service or paying down debt), articulate the business benefit. Here are a few examples:
* “This will let us handle holiday traffic spikes without downtime, protecting revenue.”
* “Improving site speed has proven to increase conversion; this could realistically add $X in sales.”
* “Refactoring this module will reduce maintenance time, freeing approximately 20% of engineering capacity for new features.”

# Balancing Vision with Execution

*Technical vision* is your idea of an ideal future state (architecture, capabilities). *Execution* is what you deliver incrementally now. Strategically, you must balance dreaming big with delivering value regularly.

Avoid spending months building the “perfect” system while delaying deliverables indefinitely. Instead, keep a long-term plan in mind, while connecting it to immediate actions:

### Have a vision.

As a senior IC, you should have some sort of technical vision for your area. For example, “In a year, I think we should move toward a microservices architecture with well-defined APIs so each team can deploy independently.” Or, “Our vision is to have zero downtime deployments and A/B testing on every feature.” Vision provides direction; it’s the North Star for technical decisions. Without it, you might make short-term choices that conflict in the long run.
