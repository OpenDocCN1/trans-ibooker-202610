
## Metrics Connected to Events: A Single Unified System

Traditionally, we think of metrics as being completely separate from logs. But they are intimately connected. For example, let’s say that an API has a metric that measures the number of errors per minute. That’s a statistic. However, each error was created by a specific transaction, using specific resources. Those specifics are present every time we increment that counter, and we want to know those specifics.

When an operator is alerted to a sudden spike in errors, their first question will naturally be “What’s causing that spike?” Looking at example traces may answer this question. Events that occurred earlier in the failed transaction (or that failed to occur) may be the source of the error.

In OpenTelemetry, when metric events occur within the context of a span, a sample of these traces is automatically associated with the metric as *trace exemplars*. This means that there’s no guessing or hunting for logs. OpenTelemetry definitively links traces and metrics together. An analysis tool built on OpenTelemetry would let you go straight to your traces from your dashboards, with a single click. And if there is a pattern—for example, a particular attribute value correlates strongly with traces that result in a particular error—this pattern could be automatically identified.

## Automated Analysis and the Single Braid

Events, resources, spans, metrics, and traces: these are all connected in a single graph by OpenTelemetry, and they are all sent to the same database to be analyzed as a whole. This is the next generation of observability tools.

Modern observability will be built on data using structures that allow analysis tools to make correlations across all types of events and aggregates, and these correlations will have a profound effect on how we practice observability.

The transition to observing our systems holistically will have many benefits. But I believe that the primary time-saving features these new tools provide will be various forms of *automated correlation detection*. When searching for a root cause, noticing correlations can

generate substantial insights. As shown in Figure 2-4, correlations are often the key ingredients of generating a root cause hypothesis, which can then be investigated further.

> "Extreme latency is highly correlated with kafka.node => 6"
> "Increased error rate is highly correlated with service.version => v1.3.2"
> "Traffic spike is highly correlated with username => mallory@sketchy.biz"

Figure 2-4. Examples of correlations that may provide a key insight, pointing to a root cause.

What does average performance look like? What do outliers look like? What trends shift together, and what do they have in common? Correlations may occur in many places: between attributes in a span, between spans in a trace, between traces and resources, within metrics, as well as in all those places together. When all of this data is connected in a graph, these correlations are ready to be discovered.

This is why a single braid of data is so critical. The value of any automated analysis hinges entirely on the structure and quality of the data being analyzed. Machines traverse graphs of data; they don’t make leaps of logic. Accurate statistical analysis requires a telemetry system that is intentionally designed to support it.

## The Point: Automated Analysis Saves You Time

Why do we care about automating correlation analysis? Because time and complexity are against us. As systems grow in size, they eventually become too complex for any operator to hold entirely in their head, and there is never enough time to investigate every possible connection when building a hypothesis.

The problem is that choosing what to investigate requires intuition, and intuition often requires a deep knowledge of each component that makes up a distributed system. As organizations grow their systems and scale their engineering workforce accordingly, the portion of each system that any individual engineer deeply understands naturally shrinks to a smaller percentage of the entire system. Intuition doesn’t scale very well.

Intuition is also notoriously misguided; problems frequently arise in unexpected places. Almost by definition, problems that can be anticipated do not occur very often. What is left are all of the unanticipated problems, problems that have already slipped past our intuition.

This is where automated correlation detection comes in. With the right data, relevant correlations can be detected more effectively by machines. This allows human operators to move quickly, repeatedly testing hypotheses until they know enough to develop a solution.

## The Limitations of Automated Analysis

Automation is beginning to sound amazing, but here’s an important reality check: at the end of the day, computer analysis will not be able to tell you what is wrong with your system or fix it for you. It can only save you time.

With that in mind, I want to stop with the praise for a moment and acknowledge some limitations of automation. I do this because there will be quite a lot of hype around combining artificial intelligence and observability. This kind of hype leads to amazing claims along the lines of “AI anomaly detection and root-cause analysis will fully diagnose your problems!” and “AI operations will fully manage your system!”

## Beware of Hype

To derail this hype train, I want to be clear that these kinds of dreamy marketing claims are not what I am claiming modern observability will provide. In fact, I predict that many claims related to intelligent AI problem-solving will mostly be a lot of snake oil.

Why won’t AI solve our problems? In general, machines cannot identify “problems” in software because defining what counts as a “problem” requires a form of subjective analysis. And the kind of real-world AI we are talking about cannot perform subjective

decision making with any degree of accuracy. Machines will always lack sufficient context.

For example, let’s say that a release slows down performance. But it also contains a really important feature that’s number one on every user’s wish list! In that case, the performance regression is a feature, not a bug.

While they might seem like similar activities, there is a vast chasm between *identifying correlations* and *identifying a root cause*. When you have the right data structures, correlations are a kind of *objective analysis*—you just have to crunch the numbers. Which of those correlations are relevant and indicate the source of a real problem will always require *subjective analysis*, an interpretation of the data. What do all these correlations mean? As Jeffrey Lebowski would say, “Well, you know, that’s just, like, your opinion, man.”

## Magical AIOps

When investigating a system, two types of analysis come into play:

- Objective analysis, which is fact based, measurable, and observable
- Subjective analysis, which is based on interpretations, points of view, and judgment

This dichotomy—objective versus subjective—relates to an important problem in computability theory called the *halting problem*. The halting problem is defined as whether a computer program could be written that could determine whether any arbitrary program would finish running or continue to run forever, given a description of that arbitrary computer program and its input. The short version is that in 1936, Alan Turing proved that a general algorithm to solve the halting problem cannot exist, and an extension of this proof can be applied to many forms of identifying “problems” in computer software.

What Alan Turing is saying is that we don’t get to have magical AIOps (artificial intelligence for IT operations). Look for tools that promise to automate tedious objective analysis—crunching numbers is what machines are good for! But beware of tools that claim to find the root cause of problems and automatically fix them; they will most likely leave you disappointed.
