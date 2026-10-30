
because new users compare that price to open source and “free” alternatives that they presumably could just build themselves. However, people are generally not accustomed to considering the price of their own time. When it takes an hour to spin up some infrastructure and configure software, it feels like a DIY solution is essentially free.

In reality, the costs of ongoing maintenance—the time burned for context switching, the opportunity cost of all those engineers devoting time to something of no core business value, all those hours being siphoned away by that free solution—are almost always underestimated. In practice, even when you think you’ve accounted for the underestimation, you’ll likely still underestimate. As humans, our gut feelings are wildly, even charmingly, optimistic on the matter of maintenance costs. As a starting point, this is both understandable and forgivable.

Even when you do proceed beyond that starting point, sometimes the urge to build a solution yourself is a matter of encountering a budgetary mismatch. Getting approval to spend money on buying software may be harder than getting engineers you already employ to spend time building and supporting an in-house alternative. However, organizations that fall into this category are often reluctant to admit just how much that free effort is truly costing their business. Overcoming a budgetary mismatch may prompt an initial decision to build, but over the long term, that decision will present substantial costs to an organization that aren’t usually being tracked after the initial decision to build is made.

To analyze the ROI of building or buying observability tools, first you must start by truly understanding the costs of both decisions. Calculating total cost of ownership (TCO) requires factoring in many considerations to be exact, but you can use general guidelines to get started.

## The Real Costs of Building Your Own

Let’s start with opportunity costs. Resources are finite, and every choice to spend dollars or time impacts what your business will be able to accomplish. *Opportunity cost* is the value of the next-best alternative when

a decision is made. By choosing to go the route you did, what did you give up in return?

Are you in the business of building observability solutions? If the answer to that question is no, building a bespoke observability tool is likely a distraction from your company’s core objectives. Therefore, the opportunity cost of choosing to build one yourself is big.

The rarest naturally occurring element on Earth is astatine (atomic number 85 on the periodic table), with less than 1 gram present deep within the Earth’s crust at any given time. The second rarest element is unallocated engineering time. We kid, of course. But if you’re a product or an engineering manager, you’re probably “lol sobbing” at that joke (sorry, managers!). In any given company, endless competing demands clamor for engineering cycles. As leaders and engineers, it’s your job to filter out the noise and prioritize the most impactful needs requiring solutions that will move the business forward.

As software engineers, it’s your job to write code. The opportunity cost problem comes into play whenever that talent is applied toward every arbitrary challenge that you might encounter. When you see a problem, if your first and foremost inclination is to just write some code to fix it, you may not be considering opportunity costs. While coding is fast and cheap, maintaining that code is extremely time-consuming and costly. As fellow engineers, we know that often we are so preoccupied with whether we *can* do something that we don’t stop to think about whether we *should*.

To calculate opportunity costs, you must first calculate the financial costs of each option, quantify the benefits received with each, and compare the two. Let’s start with an example of calculating the costs of building your own.

## The Hidden Costs of Using “Free” Software

You could choose to build your own observability stack using open source components that you then assemble yourself. For simplicity, we’ll presume this is the case. (An alternative build option of creating every stack

component from scratch would carry costs at least an order of magnitude higher.) To illustrate actual operating costs, we’ll use a real example.

We spoke to an engineering team manager who was considering investing in a commercial observability stack after having built an in-house ELK stack. We’d given him a quote that was about $80,000/month to provide observability for his organization. He immediately balked at the sticker shock of a nearly $1 million annual solution and pointed to the free ELK stack his team was running internally.

However, he wasn’t counting the cost of the dedicated hardware used to run that ELK cluster ($80,000/month). He also wasn’t counting the three extra engineers he had to recruit to help run it ($250,000 to $300,000 each, per year), plus all of the one-time costs necessary to get there, including recruiter fees (15% to 25% of annual earnings, or about $75,000 each), plus the less tangible (though still quantifiable) organizational time spent hiring and training them to become effective in their roles.

The seemingly free solution had been costing his company over $2 million each year, more than twice the cost of the commercial option. But he’d been spending that in ways that were less visible to the organization. Engineers are expensive, and recruiting them is hard. It seems especially wasteful to devote their talent to bespoke solutions in the name of saving a few dollars, when the reality is often much further from the truth.

Heidi Waterhouse, principal developer advocate at LaunchDarkly, muses that open source software is “free as in puppies, not free as in beer.” Indeed, in the preceding example, this engineering manager was spending twice as much to do non-mission-critical work that did not advance his company’s core competencies.

The first step to understanding ROI is to truly flesh out the hidden and less visible costs of running supposedly free software. This isn’t to say that you shouldn’t run open source software. It’s to say that, when you do, you should be fully aware of what it is really costing you.

## The Benefits of Building Your Own

In many organizations, the deciding factor comes down to justifying visible spending. When budgets and expectations are mismatched, justifying spending time and hiring additional developers to build and support an in-house solution is often easier than creating a new line item to pay to acquire software externally. While the true estimation of costs in those cases is misguided, building your own software provides an additional benefit that may balance the equation a bit.

Building your own software also builds in-house expertise. The very act of deciding to build your own custom approach means that it becomes someone’s job to deeply understand your organizational needs and turn those into a set of functional requirements. As you work with internal stakeholders to support their needs, you will learn how to bridge the gap between technology and your business.

When building your own observability solution, you will inherently need to internalize and implement many of the solutions detailed in this book—from the necessary data granularity provided by events, to automating the core analysis loop, to optimizing your data store to performantly return results against large quantities of unpredictable high-cardinality data. But you will deeply internalize those solutions as they’re implemented specifically to meet your organizational needs.

You’ll learn how to make distributed tracing valuable for the needs of your own particular applications through numerous trials and many useful errors working with your own developer teams. You’ll learn exactly where metrics are serving your business and where they’re not by slamming headfirst into the limitations of their coarseness in the debugging loop. You’ll learn how to adjust error budget burn alert calculations for your implementation SLOs as you miss timely responses to production outages.

Over time, you will so deeply understand your own organizational challenges that you will build a team whose job it is to write instrumentation libraries and useful abstractions for your software engineering teams. This observability team will standardize on naming conventions across the organization, handle updates and upgrades
