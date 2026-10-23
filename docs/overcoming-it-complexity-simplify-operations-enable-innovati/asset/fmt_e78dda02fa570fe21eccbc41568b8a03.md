
*   Analyze our changes to see whether they’ve made things better or worse, or improved things enough to justify the cost of the change.
*   Determine when it is time to stop attempting to make improvements.

# How Deep Do You Measure?

Earlier I gave the example of a large enterprise spending six months on a large, formal system audit. This is a form of measurement. It will give a complete view of the state of the system that is accurate and highly detailed—but as depicted in [Figure 2-3](#ch02.html#fig_3_a_formal_assessment_is_inaccurate_the_moment_it_s), it will show the system state as it was six months ago, when the audit started.

![](assets/oitc_0203.png)

Such an audit is not of any use to us in managing systemic improvements, when we are trying to incrementally improve between each measurement cycle. If we can only make small changes every six months at the end of a formal audit/measurement cycle, then we can’t make changes very quickly, and our entire Measure-Try-Measure-Refine process fails. In order to confront the IT complexity dilemma, we need to break out of these long, unwieldy, and expensive evaluation cycles. The duration of our Measure-Try-Measure-Refine loop should be measured in days or hours, not in months or years.

So, when you create an inventory of your system, when you conduct a review of your applications, when you *measure* the current state of your application and its infrastructure, how deep should you measure the system?

One viewpoint says that you need to have a deep and detailed view into your application and its infrastructure. You need to know about every CPU, data memory chip, network segment, cable, application procedure, service, node, etc. If you don’t know everything, you know nothing. This viewpoint is what leads to the six-month formal external audits we talked about previously. Requiring precise measurement of everything means you can’t possibly know everything you need to know in a timely manner. It can also lead to knowledge without understanding: by the time you have measured everything, you have no context for applying the understanding you’ve gained.

Too much data isn’t always helpful, especially if the cost of getting that data makes the data inherently less useful. If it takes you six months to collect the data, you can’t use that data to determine what changes you need to make *today* to make things better *tomorrow*.

Instead, you will need to compromise. You’ll need to collect some subset of the data, with the expectation that the subset you collect gives you the insight you need to extrapolate the rest of the data. [Figure 2-4](#ch02.html#fig_4_rather_than_doing_a_full_assessment_you_can_do_a) illustrates this.

![](assets/oitc_0204.png)

But what data do you need? Let’s assume you have a large system that is running many large applications and you want to do an inventory that lets you know how many infrastructure components are needed by each application. You are doing this so you can compare these numbers to the amount of inventory you actually have on hand and understand whether you have excess capacity or are running your services too lean. Taking a complete inventory would be too expensive and take too long—but how can you determine what you have without doing a full inventory of the entire system? Let’s look at some possibilities.

*Option 1: Look at only some attributes of the inventory (such as compute) and ignore others (such as networking or storage)*.

One option is to look at only a subset of your infrastructure. For example, you might look only at the CPUs. How many CPUs do you have? How powerful are they? How much raw computation power does that represent? You can then determine how much computation power you have assigned to each of the parts of each of your applications.

In focusing only on raw computation power, you’re ignoring other important aspects of the infrastructure an application requires, such as memory, storage, and network resources. The extrapolation you make is that if one application has twice the compute resources as another, you can assume it also has twice the networking, memory, and storage resources.

However, this assumption is rarely accurate. Just because an application has twice as many CPUs assigned to it as another, or has CPUs that are twice as powerful, does not mean that those CPUs have twice as much memory, or twice as much database storage, or twice as much networking capacity.

Using one type of resource as a proxy for your entire system inventory leads to inaccurate data and bad expectations.

*Option 2: Look at a single application or service in its entirety, and ignore all other applications/services*.

Another approach you can take when creating your inventory is to look at a subset of your applications or services. For example, you might decide to investigate a single application in its entirety. You determine how much computation it requires, and how much memory, storage, networking capacity, etc. You figure out what services compose this application, and what external services the application requires to operate. You make a complete inventory of everything required to run this one particular application.

Then you make the assumption that all the other applications or services in your system will have a similar set of requirements, based on some point of comparison. For example, if your analyzed application needs 25 servers and another application needs 50 servers, you can assume that the other application uses twice as much *everything* as your analyzed application.

This is perhaps a bit more accurate than option 1, but as you can imagine, it is still inaccurate. How much infrastructure a given application or service requires does not have much bearing on how much another application or service requires. Any two applications will likely look very different internally, and may use a very different mix of support services. You are still no closer to understanding the complete infrastructure needs for your entire system.

So, if neither of these options is a very useful model for getting a complete picture of the inventory needs of your system, how can you do that, short of performing a full, multimonth audit of your entire infrastructure?

The options considered here attempted to speed up the inventory process by pulling out a subset of important data from the dataset and extrapolating from that. But as we saw, this resulted in inaccuracies. Instead, how about simply spending less time creating the inventory list in the first place? Rather than trying to accurately and completely create an inventory of a small section of your infrastructure, you can create a general overview inventory that may not be very accurate, but will represent your entire system. I refer to this as an *adaptive assessment*.

# Adaptive Assessment

Let’s accept the fact that this may not initially be a very accurate inventory. Instead, as time goes on and you adjust your system, you’ll add more data to your existing (incomplete and inaccurate) inventory, improving it little by little. That is, with each change, you’ll *adapt* your inventory, adding more data to it and making it a slightly more accurate overall assessment of your system.

With the adaptive assessment approach, rather than striving for perfection in complete inventory (which will take too long) or part of the inventory (which will be unrepresentative), we *estimate* the inventory of the entire system. Then, over time, we refine the estimate and make it more and more accurate.

It’s an iterative approach to inventory assessment. [Figure 2-5](#ch02.html#fig_5_an_adaptive_assessment_grows_and_expands_with_your) illustrates this process. In the adaptive assessment, you are trading some amount of inaccuracy for speed. In other words, you are exchanging *granularity of assessment* for development speed.

![](assets/oitc_0205.png)
