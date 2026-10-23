
![](img/51e9508eebbc565953ed15bd4b5bec8f_185_0.png)

## 7.7.3 Helping customers choose content

The final exercise is about Netflix’s personalized home page of video content. In general, the vision is about “helping consumers find content to watch on the Netflix platform.” Here’s the business goal:

> *Reduce the amount of time viewers spend choosing what to watch.*

A natural way to find a KPI for this goal is to compare the performance of the personalized home page with a random choice of content on the Netflix platform. This is a great way to immediately track the impact of the project, and one that is going to make it hard for the rest of the organization to ignore your project. This is how this choice looks:

> *Users who browse a personalized home page are quicker to choose which content to watch.*

And here are the corresponding acceptance criteria:

> *Users who browse a personalized home page are 20% faster at choosing what to watch compared to a randomized list.*

In general, you should always try to tie the success of your project to user behavior metrics, as that’s the most direct way to measure the impact of your efforts. This concept is widely known as *A/B testing*, because you’re comparing a new variant of a feature or product (based on your new AI model) with the status quo. The best candidates for A/B testing are projects for which you have a constant flow of users or items, and thus you can afford to split them into two groups: one that you send to the old system, and one that you send to the new system. By comparing performance across the two user groups, you’ll have the best idea possible of how your project is going.

Moving on to the ML Outputs section of the Canvas, we'll need to be specific about how the output of the ML model is going to be used to personalize the home page. Here's the most straightforward option:

> *A list of the five movies that the user is most likely to enjoy*

But keep in mind that, as you saw at the end of chapter 6, a real-world implementation will have several distinct lists to increase its chance of grabbing the attention of the user. We're sure you recognized that this example is a typical application of a recommendation system, which means that the ML inputs are going to look like this:

> *The viewing history of the target user and all of the others on the platform.*
> *Information about the movies (director, year, genre, and so on).*

Where are you going to be collecting this data from? We don't have a lot of options here:

> *Viewing history collected from video servers. Movie information collected from internal databases.*

In fact, this example is typical: you won't have to look very far for the data assets, because they're already crucial to the operation of the organization, and thus likely to be collected and managed carefully. Trivially, any video-streaming platform already stores information about movie genres, actors, and directors so that it can use that data when presenting movies to users. It's no big stretch to take this information and use it for machine learning. Likewise, viewing history is also readily available because it must be tracked for accounting purposes (for example, to pay out royalties). Figure 7.12 shows the completed Framing Canvas for this exercise.

We covered a lot of material in this chapter. You learned the difference between a long-term AI vision and individual AI projects, and how the latter helps your organization shape the former. We also explored the attributes that make AI projects valuable

![](img/51e9508eebbc565953ed15bd4b5bec8f_186_0.png)

for organizations, and warned you about the potential risks and pitfalls encountered when rolling out a new project. Finally, we introduced the Framing Canvas as an important tool that helps you distill “business speak” into “machine learning speak.”

The next chapter will show you how to take this ML-friendly definition and put together a team and plan that your organization can use to implement your project.

## Summary

- Your organization’s AI vision is a clear path that defines how it will look in the AI era, how it will help its people work more efficiently, and how it will shape your products to serve your customers.
- An AI project is a well-defined initiative that uses AI to build new products or features, or to optimize existing processes.
- Some complex AI projects need to be divided into independent ML tasks, defining the KPIs, the inputs and outputs, and the data sources you intend to use. The Framing Canvas helps you do that.
- The Framing Canvas helps you phrase the goals and vision for an AI project in ML terms.

## Set—preparing data, technology, and people

### This chapter covers

- Identifying potential sources of data, both inside and outside the organization
- Assessing the quality and quantity of data
- Assembling an effective AI team

This chapter picks up where chapter 7 left off. Now that you know how to use the Framing Canvas to create an ML-friendly vision for your project, it’s time to put together the other ingredients that you need. Bringing a project to life requires three main ingredients: the ML model, data, and people. While choosing a good ML model is a task for the technical team, it’s your job to recruit them and craft a data strategy so they can get to work. This chapter focuses on how to find and manage data, and how to recruit a team of talented people with the right skills for your project.

## 8.1 Data strategy

One of our goals when writing this book was to make you think critically about data and understand how engineers use it to build ML models. Because data is so crucial, developing a coherent data strategy is critical for the success of any project. In part 1, we talked about data in a way that took for granted that you had it readily available to build your models. You can probably guess that this is rarely the case; this chapter will fill in the gaps and help you understand how much data you need, where you can get it, and how to manage it.

One distinction we have to make is between the data strategy of your organization and the data strategy of your AI project. When business media or executives talk about a data strategy, they’re generally talking about the overall company-wide strategy for acquiring, storing, and using data. This strategy is driven by the company’s long-term goals and vision for the future.

This book focuses on the data strategy of your project, which is specific to a single AI initiative. Having a project-specific focus is better than a broad one for two reasons.

First, developing a data strategy even for a single project forces you to be concrete about the specific data you’ll need to collect to build your project, how much you’ll need, and where you’re going to get it. To develop an AI strategy, you may say, “We’ll build infrastructure to collect data from users’ interactions with the platform.” But to build an AI project, you need to say, “We’ll use user clicks, the time they spend on each page, and social shares, and merge that with the CRM data to recommend content.” This level of specification helps you think about what you really need to collect to build your projects. Someone has to make the decision of what exactly to collect, and if the directions you give the IT team are too broad, they’ll make that call for you, often without knowing the business context.

The second (and more important) reason to think about an AI project data strategy is that the organization-wide strategy should be informed by the needs of each AI project that you’re planning to pursue. Starting to think about a company-wide data strategy without any experience is like starting to think about your family house when you’re single. You simply don’t know what you’ll need. (Will you have zero, one, or five kids? Which location is more convenient for you and your partner? What’s your partner’s taste? What can you afford?) Just as individual AI projects help you form a more informed AI vision, likewise building up your data strategy as the minimal set of data needed for the projects helps you make sure you are collecting the data you need.

If you start from AI projects instead, building the organizational data strategy will be a simple exercise of combining the experience that you gained through the implementation of each project (as shown in figure 8.1).

To build the data strategy for an AI project, you need to know where to get data and how much you need. The next two subsections dive into each topic.
