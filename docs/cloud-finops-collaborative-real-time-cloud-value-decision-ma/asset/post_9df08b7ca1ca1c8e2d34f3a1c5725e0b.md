
Let’s say, for example, that FinOps says doing something will result in large amounts of savings, but it will require significant effort. In a case like this, first confirm that the level of effort (LOE) does not exceed the potential savings, then explain the level of effort required and ask FinOps to negotiate dedicated time with your manager.

You can negotiate for extra people, extra projects, and efforts you think are important to you and your group. In many cases, the central FinOps team receives pushback, but they are often asking: “Can you help me help you?” You can counter-negotiate with resources or say, “Can I perform the top five recommendations versus the whole list of recommendations? Alternatively, can we defer this task two quarters from now to better fit your schedules?”

You might say these are all things for your manager to think about, but FinOps is a new concept, and FinOps teams need champions like you to help explain it to their managers. Sure, we know that we are inundated with a huge list of tasks, and the last thing you want to do is to add more items to that list. But understand that your manager is no different, and they’re seeking your help to understand and assess how to move forward as a group. The payoffs are potentially great. Not only will you help your own organization, but you can take the skills you learn in managing costs to another company—helping you with your own career ambitions.

As an engineer, I suggest that you keep a running tally of costs you’ve saved the company. Finance and the business teams already do this, but you keep track of this data and you might find yourself quickly becoming a leader in this space as there aren’t a lot of engineers fighting for the cause. Accountability works both ways, so hold others to the same level of standards that you hold yourself.

The key question finance partners should ask engineers is: “Do you have everything you need?” The key questions engineers should ask business or finance is: “Are the cost-saving changes going to be greater than the cost of the engineering effort it takes to make the changes? What other options can I explore to make this successful?”

Your job is to balance the cost versus the benefit, then make a decision. If someone asks you to clean up something small, it can be done as a five-minute effort of good-will. Or it may be simply that a call was made to deploy suboptimally on purpose and it will be ironed out in the next release. Whatever you decide, be accountable: always do everything in your power to follow up your commitments with action.

## 4: Introduce Financial Constraints Early in the Product Development

If you introduce strong financial constraints at the end of the development cycle, do not expect them to be in the next release. Quick wins might move the needle in the right direction, but they are unlikely to get you to the long-term goals (see the Afterword of the book for more on prioritizing long-term gains over short-term wins). Financial constraints take time to implement, especially if introduced late in the game, as they may not be compatible with fundamental choices made early on, requiring rearchitecting. The cost to rearchitect may ultimately not be worth the cloud cost savings when compared to the engineering time required to do so and its impact on delivery schedules of other releases. As Gabe said in his FinOps X talk: “If finance only shows up at the point of a release hitting staging or production, then it’s too late. You need to bake cost constraints into the beginning, otherwise you’re often asking engineering to build a new product.”

## 5: Enablement, Not Control

Requirements are the main thing engineers focus on. Their job is first to cut out the noise of the requirements to determine what really needs to be built. Then they examine the constraints to determine how to build it. If you have overly rigid restraints, you end up with what Gabe calls *technicians*: those who are told to simply “turn the valve this much” without ever innovating. On the flip side, you have what Gabe called *artists*, who go too far in crafting artisanal and usually overly expensive solutions to a problem. There’s a classic episode of *The Simpsons* where Homer is given free rein and an unlimited budget to design the perfect car. What comes out the other end is an expensive monstrosity. Without finding an appropriate level of cost guardrails and other constraints, innovation can actually suffer.

In the middle there are enough constraints to be creative, but it’s not so rigid that you can’t find an elegant solution. Simple guardrails like a total cost or a list of approved cloud service SKUs or approved regions can create innovation. But if you do that, you need to provide other parameters where engineers have flexibility. For example, if you stay within this budget and avoid these services or regions, then you can do whatever you want.

On this principle, Gabe ended by sharing that “we are so worried about artists but we also don’t want technicians. Too many constraints turn into restraints. Enable people to do the good behavior you want. And remember that all engineers do not know everything about the cloud; education is key.”

## 6: Leadership Support Isn’t Helpful, It Is Essential

Leadership is the single most important factor of the success of a FinOps transformation, or any organizational transformation for that matter. If your engineering and finance leaders don’t agree on the importance and the desired outcomes, then the cultural change is doomed to fail. Again, it’s not that you need mandates from leadership but that you need leaders to constantly send signals that cost—and its ultimate outcome of business value—is important at all stages of development. The message should not be a mandate such as “reduce cost by 10%,” but rather a constant drumbeat that “cost efficiency is always important and is a core part of your job.”

Here Gabe referenced his former job as a security engineer, saying, “You can’t just do occasional security sprints; you’re always baking security into every part of the development lifecycle. If a new feature breaks your cost constraints then it’s a bug, just as it would be with security. You have to apply that level of discipline.”

## Stories from the Cloud—Mike

My central FinOps team has evolved their approach over the years to work more closely with the hundreds of engineering partners we have at Atlassian.

We go to them now with a specific ask, such as: “We want to commit to some Savings Plans or Reserved Instances to achieve this specific goal. Here’s the commitment that we are considering, here’s the value that we expect it to return, here’s what it will do for the business. Do you have any blockers to us moving forward?”

Over the years, more attention and time was given to us as we became better at focusing on value conversations and the FinOps team became more established. In the beginning, we were simply focused on best practices. I spent a lot of time trying to convince folks informally to think more about cost.

Once we solidified ourselves as a dedicated team with broad leadership support, it made it more obvious to the larger organization that FinOps is not just a side thought for Atlassian, as it was in the early days. They see that our team is being funded as a key investment by leadership, and this helps drive the message that they need to partner with us.

At the same time, we come in cautious about what we’re trying to achieve. We make sure they understand the ask, what they need to do, and what the impact of that effort is.

The more challenging conversations happen when a team has made a budget commitment and now they aren’t meeting it. In that case, we’ll come in with questions like:

- Why are we not hitting the things we committed to?
- Is this a one-off?
- Is this a new trend?

We try to come up with a solution with them by asking questions like:

- Can you change some other things in order to hit the commitments?
- We see growing waste recommendations; can we slot some time to tidy them up?
- What can we change to reduce the likelihood of resources becoming waste?

When we need to be the police, we come in with information or data that proves there’s something to be concerned about. Anomalous spend, an exceeded budget, or a forecast that shows that we expect to exceed the budget considerably. We ask questions like: this data indicates something is wrong, can you help us understand it? This slowly builds the rapport so that—instead of booking a meeting—we can eventually send a team a quick asynchronous question and get a cooperative response.
