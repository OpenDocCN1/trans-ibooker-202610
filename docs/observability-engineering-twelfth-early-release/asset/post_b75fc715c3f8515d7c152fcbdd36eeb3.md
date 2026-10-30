
debugging conditions are completely unknown to you, then you must instead debug from first principles.

## Debugging from First Principles

As laid out in the previous two chapters, gathering telemetry data as events is the first step. Achieving observability also requires that you unlock a new understanding of your system by analyzing that data in powerful and objective ways. Observability enables you to debug your applications from first principles.

A [first principle](first principle) is a basic assumption about a system that was not deduced from another assumption. In philosophy, a first principle is defined as the first basis from which a thing is known. To *debug from first principles* is basically a methodology to follow in order to understand a system scientifically. Proper science requires that you do not assume anything. You must start by questioning what has been proven and what you are absolutely sure is true. Then, based on those principles, you must form a hypothesis and validate or invalidate it based on observations about the system.

Debugging from first principles is a core capability of observability. While intuitively jumping straight to the answer is wonderful, it becomes increasingly impractical as complexity rises and the number of possible answers skyrockets. (And it *definitely* doesn’t scale…not everyone can or should have to be a systems wizard to debug their code.)

In Chapters [2](2) and [3](3), you saw examples of elusive issues that were incredibly difficult to diagnose. What happens when you don’t know a system’s architecture like the back of your hand? Or what happens when you don’t even know what data is being collected about this system? What if the source of an issue is multicausal: you’re wondering what went wrong, and the answer is “13 different things”?

The real power of observability is that you shouldn’t have to know so much in advance of debugging an issue. You should be able to systematically and scientifically take one step after another, to methodically follow the clues to find the answer, even when you are unfamiliar (or less familiar) with the system. The magic of instantly jumping to the right conclusion by inferring an unspoken signal, relying on past scar tissue, or making some leap of familiar brilliance is instead replaced by methodical, repeatable, verifiable process.

Putting that approach into practice is demonstrated with the core analysis loop.

## Using the Core Analysis Loop

Debugging from first principles begins when you are made aware that something is wrong (we’ll look at alerting approaches that are compatible with observability in Chapter 12). Perhaps you received an alert, but this could also be something as simple as receiving a customer complaint: you know that something is slow, but you do not know what is wrong. Figure 8-1 is a diagram representing the four stages of the core analysis loop, the process of using your telemetry to form hypotheses and to validate or invalidate them with data, and thereby systematically arrive at the answer to a complex problem.

![](img/aaaa2d27e08e754554d501192f304d8f_133_0.png)

*Figure 8-1. The core analysis loop*

The core analysis loop works like this:

- 1. Start with the overall view of what prompted your investigation: what did the customer or alert tell you?
- 2. Verify that what you know so far is true: is a notable change in performance happening somewhere in this system? Data visualizations can help you identify changes of behavior as a change in a curve somewhere in the graph.
- 3. Search for dimensions that might drive that change in performance. Approaches to accomplish that might include the following:
  a. Examining sample rows from the area that shows the change: are there any outliers in the columns that might give you a clue?
  b. Slicing those rows across various dimensions looking for patterns: do any of those views highlight distinct behavior across one or more dimensions? Try an experimental group by on commonly useful fields, like `status_code`.
  c. Filtering for particular dimensions or values within those rows to better expose potential outliers.
- 4. Do you now know enough about what might be occurring? If so, you’re done! If not, filter your view to isolate this area of performance as your next starting point. Then return to step 3.

This is the basis of debugging from first principles. You can use this loop as a brute-force method to cycle through all available dimensions to identify which ones explain or correlate with the outlier graph in question, with no prior knowledge or wisdom about the system required.

Of course, that brute-force method could take an inordinate amount of time and make such an approach impractical to leave in the hands of human operators alone. An observability tool should automate as much of that brute-force analysis for you as possible.

## Automating the Brute-Force Portion of the Core Analysis Loop

The core analysis loop is a method to objectively find a signal that matters within a sea of otherwise normal system noise. Leveraging the computational power of machines becomes necessary in order to swiftly get down to the bottom of issues.

When debugging slow system performance, the core analysis loop has you isolate a particular area of system performance that you care about. Rather than manually searching across rows and columns to coax out patterns, an automated approach would be to retrieve the values of all dimensions, both inside the isolated area (the anomaly) and outside the area (the system baseline), diff them, and then sort by the difference. Very quickly, that lets you see a list of things that are different in your investigation’s areas of concern as compared to everything else.

For example, you might isolate a spike in request latency and, when automating the core analysis loop, get back a sorted list of dimensions and how often they appear within this area. You might see the following:

- `request.endpoint` with value `batch` is in 100% of requests in the isolated area, but in only 20% of the baseline area.
- `handler_route` with value `/1/markers/` is in 100% of requests in the isolated area, but only 10% of the baseline area.
- `request.header.user_agent` is populated in 97% of requests in the isolated area, but 100% of the baseline area.

At a glance, this tells you that the events in this specific area of performance you care about are different from the rest of the system in all of these ways, whether that be one deviation or dozens. Let’s look at a more concrete example of the core analysis loop by using Honeycomb’s BubbleUp feature.

With Honeycomb, you start by visualizing a heatmap to isolate a particular area of performance you care about. Honeycomb automates the core analysis loop with the BubbleUp feature: you point and click at the spike or anomalous shape in the graph that concerns you, and draw a box around it. BubbleUp computes the values of *all* dimensions both inside the box (the anomaly you care about and want to explain) and outside the box (the baseline), and then compares the two and sorts the resulting view by percent of differences, as shown in [Figure 8-2](Figure 8-2).

# Distribution of Events failing SLI by duration_ms

Make Default

8/5 6:00am to 8/5 12:00pm

Aug 5 2021, 6:00 AM – Aug 5 2021, 12:00 PM

Interesting area

# Dimensions

Try to GROUP BY columns that look most different between the successful and failed.

# 63 interesting dimensions found (ranked)

1-15 of 63

# Measures

Try asking WHERE the successful and failed are most different.

1-5 of 9

Baseline values constructed from 10000 samples Selection values constructed from 10000 samples

Figure 8-2. A Honeycomb heatmap of event performance during a real incident. BubbleUp surfaces results for 63 interesting dimensions and ranks the results by largest percentage difference.

In this example, we’re looking at an application with high-dimensionality instrumentation that BubbleUp can compute and compare. The results of the computation are shown in histograms using two primary colors: blue for baseline dimensions and orange for dimensions in the selected anomaly area (blue appears as dark gray in the image, and orange as lighter gray). In the top left-corner (the top results in the sort operation), we see a field named `global.availability_zone` with a value of `us-east-1a` showing up in only 17% of baseline events, but in 98% of anomalous events in the selected area (Figure 8-3).
