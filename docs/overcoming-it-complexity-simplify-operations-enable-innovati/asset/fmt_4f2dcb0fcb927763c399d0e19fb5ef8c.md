
How do you avoid the IT complexity dilemma? First things first: to deal with complexity in your application or your IT organization, you need to understand where it comes from.

The first step in understanding complexity is to perform an audit of your application, your teams, your delivery and operations processes, your IT organization, and your company as a whole, to determine what parts of your system contribute to your excess complexity.

# Auditing Versus Assessment

Auditing and assessment are two distinct terms that are often used somewhat interchangeably to describe the process of understanding the components that make up a complex system, such as an enterprise application. But what’s the difference between the two?

*Auditing* is typically defined as the process of creating a *controlled inventory*. In this context, the word *controlled* implies governance. For example:

* A bank can count its money, but it has its records formally reviewed by an independent agency for accuracy when it undergoes an *audit*.
* A company keeps its own financial records, but if it is about to be acquired or undergo a merger, its records are *audited* independently to ensure they are accurate.
* If you live in the United States, you keep track of your personal finances and you submit records to the Internal Revenue Service every year to specify how much income tax you owe. Occasionally, the IRS *audits* individuals to validate that the information they are providing is correct and accurate. Other nations have similar processes.

In an IT audit, we are talking about determining, formally or informally, the components of our applications, the infrastructure they are running on, and the systems and processes they utilize.

Large enterprises may invest hundreds of thousands of dollars in such an audit, often hiring an outside auditing firm, and the process might take six months or more. Alternatively, an architect might create a quick diagram in Visio, print it out, and put it in a logbook. Both are essentially audits, but the former is much more formal than the latter.

*Assessment* is what happens next. Once you know what components make up your systems and applications, you perform an assessment to understand how they work together, what each component is used for, why it exists, how it’s important, and the overall impact it has on the system as a whole. Often, this is part of the audit, but it doesn’t have to be. Assessing often implies grading, scoring, or evaluating. For example:

* A teacher creates a test as an *assessment* to see whether their students understand the material they were taught.
* A coach evaluates how an athlete performs to *assess* how they can utilize the athlete in a team setting.
* A voter *assesses* the pros and cons of each side of an issue before casting their vote.

Auditing and assessing are complementary processes that can be employed together to determine and evaluate the makeup of a large, complex enterprise application infrastructure.

When applied to IT applications, auditing and assessing are more akin to a *survey*. A survey is a measurement tool that provides a view of the structure and architecture of a system, and that is built and maintained outside the system. It gives us an indication of how our application, infrastructure, business, or system operates and how it’s structured.

# What Do You Measure?

In business, including application development and IT infrastructure, our measurements are built around people, processes, and technology:

People

: 

Do we have the right skill sets in the right places to allow our business to function successfully? Are our employees engaged and satisfied? Are we utilizing our people most effectively?

Processes

: 

Do we make good and timely decisions using the right data? Are our business processes efficient and effective? Do we use our time or resources inappropriately?

Technology

: 

Do we have the right technologies for our business to function optimally? Are those technologies running in the right infrastructure? Are we properly utilizing all aspects of our infrastructure? Do we waste technology we have purchased by not using it effectively? Are we inefficient because we have not acquired a piece of technology that could help us?

Determining what to assess is important but highly case dependent. Focusing on questions like those shown here is a good way to brainstorm what to measure. This gives you a great perspective on what to look at when conducting your survey.

# Why Do You Measure?

You can’t track how your organization progresses in its growth without understanding the state of the organization and how it’s currently functioning. To determine where you can improve, you must measure where you are currently.

## Measure-Try-Measure-Refine

Before you make any change to how a system operates, you need to determine how that change might affect the system. In order to do that, you need to measure the system’s current state. Then, after making the change, you can measure again and assess the impact. This allows you to refine your attempt and measure again. The result is a loop, called the *Measure-Try-Measure-Refine* loop, illustrated in [Figure 2-1](#ch02.html#fig_1_the_measure_try_measure_refine_loop).

![](assets/oitc_0201.png)

This is a basic process of cyclic improvement, and it goes by many other names. It’s very similar to the Plan-Do-Check-Act (PDCA) or Plan-Do-Study-Act (PDSA) cycle, otherwise known as the [Deming cycle](https://oreil.ly/L6Z3h).

In this version, we start at the top with the *Measure* step. We measure our system to understand its current state before making any changes.

We then move on to the *Try* step. Here, we attempt a change to see whether it will positively or negatively impact our system. Does it help us or hurt us?

To answer that question, we then do another round of *Measure*. We measure our system again and compare the current state with the original state to see how it has changed. This allows us to determine whether what we tried improved our situation or made it worse.

We then *Refine* our attempt to account for the problems or expand on the benefits. The net result should be that we are in an improved situation.

This leads us back to the top, where the cycle begins all over again: Measure-Try-Measure-Refine, then repeat. This is the process of continuous improvement.

The key point is that we must measure before and after we make any change and determine the difference between the two measurements in order to evaluate the impact of our attempt. You can’t tell if you’ve improved unless you know your situation before and after every change you make.

## The Benefits of Measurement

Every change has a cost associated with it. Those costs may be tangible (engineering costs, testing costs, the operational impact of a change, etc.) or intangible (e.g., opportunity costs). Sometimes we make a change that improves our situation, but the cost outweighs the value of the improvement. We may have made a *localized improvement*, but overall we are not better off because the cost was too great. Only by measuring can we understand the real value of the change we made, as well as the associated cost.

Another benefit of measurement is that it lets you know when you’re done. The cycle could go on forever, but sooner or later the improvements you are able to make will no longer be worth the cost of making them. As [Figure 2-2](#ch02.html#fig_2_measure_to_determine_when_it_s_time_to_stop) illustrates, measurements will tell you when to stop: when you have reached your goal, or when you have made as many improvements as you can without incurring unreasonable costs or burdens.

![](assets/oitc_0202.png)

In summary, measurement helps us:

* Determine our current system’s state and where we are in our process so we know where to focus our energies to improve.
