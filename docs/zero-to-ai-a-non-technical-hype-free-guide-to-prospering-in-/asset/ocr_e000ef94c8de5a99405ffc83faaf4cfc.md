In some cases, your only option is to pay people to label examples. A common pattern for labeling data is to use a crowdsourcing platform such as Amazon Mechanical Turk, which gives you on-demand, paid-by-the-minute access to a temporary workforce distributed across the globe. With Mechanical Turk, the only thing you can count on is that contractors will have an internet connection: because they're usually untrained, you have to prepare training materials and a labeling interface the worker can use to choose the correct labels for the training examples. Figure 8.3 shows an example of two labeling interfaces for collecting labels for image classification and object localization tasks.

![](bbox=[0.19, 0.25, 0.72, 0.4])

In general, crowdsourcing platforms are good for labeling tasks that don't need much training. If you're working on a project that requires high-level human reasoning (say, finding cancer cells on microscope scans), you would be better off putting together your own highly skilled workforce.

Table 8.1 summarizes the cost and time required to collect labels for the three labeling strategies.

Table 8.1  Cost and time requirements for the three labeling strategies

| Labeling strategy | Cost | Time required |
| --- | --- | --- |
| Natural (free) labels | Zero—You are collecting these labels already. | Zero—You already have them. |
| Hacked labels | Low and fixed—You just need to set up new data collection processes. | Depends on your traffic. |
| Paid labels | High and variable—You pay based on the number of labels you want. | Depends on the time needed to label an example and the number of labelers you have. |

Once you have a clear view of where to get the data you need, the next step is figuring out how much you need.

# ### 8.1.2   How much data do I need?

In our experience as consultants, we've often seen people falling into the trap of *big data*. It's reassuring to think that having a lot of data is a silver bullet to unlocking amazing opportunities. But as you'll see in the coming sections, data *quality* is often more important than data *quantity*.