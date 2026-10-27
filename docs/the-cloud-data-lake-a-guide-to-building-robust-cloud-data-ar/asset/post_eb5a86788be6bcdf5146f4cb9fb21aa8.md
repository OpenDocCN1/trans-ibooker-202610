
are considerations that you need to understand to effectively tune your data lake implementation to scale 10×. In Chapter 5, we'll build on these fundamental concepts of scale to optimize for performance.

## Optimizing Cloud Data Lake Architectures for Performance

> *Simplicity is the ultimate sophistication.*
> 
> —Leonardo da Vinci

*Performance* in its simplest terms can be defined as the timeliness of work completed. Having said that, this is probably one of the most loaded terms when it comes to cloud services, simply because there is no single measure for performance. In this chapter, we will peel back the layers of performance, building a good understanding of what performance means, the various dimensions associated with measuring performance when it comes to a cloud data lake, and the strategies that help optimize and tune your cloud data lake for the best performance. We will also use Klodars Corporation to illustrate these concepts and strategies.

### Basics of Measuring Performance

When time and throughput. To measure performance, we need to set a goal. In the sandwich-making example, the goal is to make five sandwiches. Next, we need to identify a set of measures that are relatively easy to measure. For the purpose of simplicity, we will assume that all the workers have the same speed when making sandwiches, and there is no transition time between tasks. We will consider variable and constant measures:

Variable measures

The following measures are variables—that is, the things you can tune or change by adding or removing:

- Number of workers = five
- Number of toasters = one toaster with space for four pieces of bread
- Number of jars of peanut butter = one
- Number of jars of jelly = one
- Number of bags of bread = one
- Number of bags = five (for the number of sandwiches needed)

Constant measures

These are measures that stay relatively constant as you change the variable measures. In this example, the variable measure is the time it takes for each step to be completed:

- Step 1, toast the bread: 30 seconds
- Step 2, smear peanut butter for one sandwich: 5 seconds
- Step 3, smear jelly for 1 sandwich: 5 seconds
- Step 4, put them together: 1 second
- Step 5, bag them: 4 seconds

Given that there are limited resources, there will be scenarios where one person will need to wait for the resources to be freed before performing their task, so you will find that each of the five sandwiches will be completed at different times.

Now, are you wondering if the pages from a cookbook have been accidentally added to this book? If yes, fear not. In a cloud data lake architecture, there are components of storage, networking, and compute that are similar to the toasters or the jars of peanut butter, and they take a certain amount of time to perform an action, which contributes to your big data processing scenarios. By both optimizing the time taken to perform the action and controlling the units of these components, you can optimize the cloud data lake performance overall. My goal for explaining these concepts

in the context of sandwich making is to make them easy to visualize and then apply in the context of cloud data lake architectures.

### Measuring Performance

Let’s now apply the variables we’ve defined to understand how much time it takes to complete making the sandwiches.

In the end-to-end execution method, one person does all the tasks required in sequential order to make the sandwich. Let’s break down how the sandwich making will work. Person 1 and Person 2 will be able to use the toaster from the time our metaphorical stopwatch begins at zero, but the others have to wait for the toaster. Similarly, after the toasting is done, Person 1 will proceed to spread peanut butter from the jar, but Person 2 must wait for them to finish, and so on.

Let’s say that everyone starts at Time 00:00 (minute and second, pick the hour of your choice). Table 5-1 shows the time taken to complete the sandwiches.

*Table 5-1. Time taken to make the sandwiches in the end-to-end execution approach*

| Sandwich | Step 1 | Step 2 | Step 3 | Step 4 | Step 5 | Total time taken since 00:00 |
|---|---|---|---|---|---|---|
| 1 | 00:00–00:30 | 00:30–00:35 | 00:35–00:40 | 00:40–00:41 | 00:41–00:45 | 45 seconds |
| 2 | 00:00–00:30 | 00:35–00:40 | 00:40–00:45 | 00:45–00:46 | 00:46–00:50 | 50 seconds |
| 3 | 00:30–01:00 | 01:00–01:05 | 01:05–01:10 | 01:10–01:11 | 01:11–01:15 | 75 seconds |
| 4 | 00:30–01:00 | 01:05–01:10 | 01:10–01:15 | 01:15–01:16 | 01:16–01:20 | 80 seconds |
| 5 | 01:00–01:30 | 01:30–01:35 | 01:35–01:40 | 01:40–01:41 | 01:41–01:45 | 105 seconds |

In the assembly-line method, we already see that a different model is in place and the time taken would be different, where each person does exactly one task and then hands over the sandwich to the next person, forming an assembly line. In this case, when the first person toasts the bread for two sandwiches, they move on to toast the next set of bread slices, while the bread for the first two sandwiches is passed off to the second and third people, who smear the peanut butter and jelly, respectively, at the same time. Table 5-2 shows how this model affects the performance.

*Table 5-2. Time taken to make the sandwiches in the assembly-line approach*

| Sandwich | Step 1 | Step 2 | Step 3 | Step 4 | Step 5 | Total time taken since 00:00 |
|---|---|---|---|---|---|---|
| 1 | 00:00–00:30 | 00:30–00:35 | 00:35–00:40 | 00:40–00:41 | 00:41–00:45 | 45 seconds |
| 2 | 00:00–00:30 | 00:35–00:40 | 00:40–00:45 | 00:41–00:42 | 00:42–00:46 | 46 seconds |
| 3 | 00:30–01:00 | 01:00–01:05 | 01:05–01:10 | 01:10–01:11 | 01:11–01:15 | 75 seconds |
| 4 | 00:30–01:00 | 01:05–01:10 | 01:10–01:15 | 01:11–01:12 | 01:12–01:16 | 76 seconds |
| 5 | 01:00–01:30 | 01:30–01:35 | 01:35–01:40 | 01:40–01:41 | 01:41–01:45 | 105 seconds |

both Step 4 (put the pieces of bread together) and Step 5 (bag the sandwich). The end-to-end execution approach does not offer this flexibility.

With these changes, let’s see how the performance of the two architectures changes, as shown in Table 5-3 and Table 5-4.

### Table 5-3. Time taken to make the sandwiches in the end-to-end execution approach after adding resources and shifting workers

| Sandwich | Step 1 | Step 2 | Step 3 | Step 4 | Step 5 | Total time taken since 00:00 |
|---|---|---|---|---|---|---|
| 1 | 00:00–00:30 | 00:30–00:35 | 00:35–00:40 | 00:40–00:41 | 00:41–00:45 | 45 seconds |
| 2 | 00:00–00:30 | 00:30–00:35 | 00:35–00:40 | 00:40–00:41 | 00:41–00:45 | 45 seconds |
| 3 | 00:00–00:30 | 00:35–00:40 | 00:40–00:45 | 00:45–00:46 | 00:46–00:50 | 50 seconds |
| 4 | 00:00–00:30 | 00:35–00:40 | 00:40–00:45 | 00:45–00:46 | 00:46–00:50 | 50 seconds |
| 5 | 00:30–01:00 | 01:00–01:05 | 01:05–01:10 | 01:10–01:11 | 01:11–01:15 | 75 seconds |

In the assembly-line method, we already see that there is a different model in place, and the time taken is different.

### Table 5-4. Time taken to make the sandwiches in the assembly-line approach after adding resources and shifting workers

| Sandwich | Step 1 | Step 2 | Step 3 | Steps 4 and 5 | Total time taken since 00:00 |
|---|---|---|---|---|---|
| 1 | 00:00–00:30 | 00:30–00:35 | 00:35–00:40 | 00:40–00:45 | 45 seconds |
| 2 | 00:00–00:30 | 00:30–00:35 | 00:35–00:40 | 00:40–00:45 | 45 seconds |
| 3 | 00:00–00:30 | 00:35–00:40 | 00:40–00:45 | 00:45–00:50 | 50 seconds |
| 4 | 00:00–00:30 | 00:35–00:40 | 00:40–00:45 | 00:45–00:50 | 50 seconds |
| 5 | 00:30–01:00 | 01:00–01:05 | 01:05–01:10 | 01:10–01:15 | 75 seconds |

With the tuning, we were able to bring the total time taken to make the sandwiches down to 75 seconds in both the architectures, and the 60th and 80th percentiles dropped to 50 seconds. Although both architectures show similar results, it’s clear that the assembly-line approach is a lot more flexible in terms of tuning compared to the end-to-end execution approach, where the only way to improve performance is to add resources. Adding workers would have no effect, and shifting workers around is not possible.

Typically, this is how performance is measured, only instead of five sandwiches, think of millions or billions of operations on a cloud data lake. The 50th percentile is a proxy of the average behavior, the 75th percentile is a good proxy for perceived behavior that you want to set targets for, and the 90th percentile is a good measure of behavior under heavy load. Now that we understand these concepts, let’s take a look at how they relate to the cloud data lake.
