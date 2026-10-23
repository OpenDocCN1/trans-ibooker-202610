
![](img/51e9508eebbc565953ed15bd4b5bec8f_190_0.png)

Figure 8.1 The organizational data strategy combines the needs of each individual project (plus some incidentals thrown in for the future).

## 8.1.1 Where do I get data?

Data has been a fundamental concept throughout this book. We’ve covered various kinds of data: structured core data, images, and sound and natural language. If you want to build an AI project, an even more important distinction will affect you: data you have, and data you don’t.

Let’s start by assuming your organization has the data you need to build your AI project. Because this data is produced and owned by your organization, we’ll call this internal data. Chapter 2 already covered the value of a specific kind of internal data that we called core business data: data with a direct impact on the top or bottom line of the organization. For our real estate platform, this can be house-price data; for Square Capital, it’s the transactions of their customers; or for Google, the consumption data of its data centers (basically, its only variable cost).

Your organization likely also produces data from business processes that may not be directly linked to your main sources of income or of costs. We’ll call this kind of data ancillary data. In our real estate example, house pictures and house reviews can be considered ancillary data: still interesting and useful, but not as much as the characteristics of the houses on sale and their selling prices.

As another more concrete example, let’s consider an e-commerce platform like Amazon, Zappos in the United States, or Zalando in Europe. In their cases, core business data would be the purchases of each customer, as they’re directly correlated to company revenues. Ancillary data may be the pages that a customer visits, the emails opened, product reviews, and so on. You can build amazing projects with ancillary data, but it’s most likely not as impactful as what you can build with core business data.

Before moving on, we want to warn you that owning a dataset doesn’t mean you can use it pain-free. You may be planning to use customer data but don’t have the permission to do so—whether customers denied their permission or, as often happens, your privacy policy wasn’t written to account for the use you’re planning for now. Even if you do have permission to use data, in our experience, most people underestimate the time and challenges of going from “we have the data” to actually bringing it into the data scientist's laptop, ready to be processed. Even if you're 100% sure that your organization has the data you need for a project, our heartfelt suggestion is that you try to account for everything that can potentially slow you down in actually using it.

In one project we worked on, the legal department of a company made it so difficult to export the data we needed that we literally had to take a plane, get into their building, plug in our laptops, encrypt the data, and bring it home on a hard disk. In another project, we knew the company had the data we needed, but we still struggled to understand what the variables in the dataset meant. The problem was that the person who designed the data collection processes had retired, and we had to spend a whole month running interviews to understand the data we were looking at.

Let's now cover the case in which you don't yet have the data you need for a project. You have three options:

- Start to collect it and wait until you have it.
- Find open data or scrape it from the internet for free.
- Buy it from providers.

The first option is the slowest; sometimes it's also the most expensive, but in some cases you may not have any other choice. Let's suppose you run a brick-and-mortar clothing shop and don't have a loyalty card system or any other way to track who buys what. If a customer walks in, you don't track that his name is John and he just bought some Nike Air Jordan sneakers, size 10, for $129. If you haven't stored this information, do you think any other company has this? Obviously, in this case, your only option is establishing new processes to collect this information and then waiting until you have collected enough data.

On the other hand, sometimes your projects need data you either can't collect or don't want to make the effort to collect. The second option in this case is to use open data.

It's amazing how much free data you can get from the internet. An example is the open data that governments often open source, which you can freely use for your projects. For instance, you can use income data for your country to focus your marketing on the wealthiest areas. Here are some other great places to look for open datasets:

- *kaggle.com*—A website where companies or individuals can host ML competitions and upload datasets.
- *arxiv.org*—A free repository of scientific papers. As researchers collect new datasets, they write and release scientific papers to present them to the scientific community and often release their datasets as well.
- *github.com*—A repository of open source code.

When using third-party datasets, you should be concerned about the quality of the data and legal issues. Many of these datasets are produced on a “best effort” basis and collected as needed to support new algorithms or fields of application; often data quality is not guaranteed. Also, many publicly available datasets are released with a noncommercial license, meaning that they can’t be used for business purposes; always check that you can use an open dataset for the purposes you intend.

The third option is buying data from providers. This strategy can sometimes be extremely expensive, and can also put you in a dangerous position as you may end up depending on these providers forever. Before deciding to go this route, we suggest you first spend enough time evaluating the associated long-term costs and determining whether multiple providers exist to buy it from. If there’s just one, you need to find a strategy to protect yourself in case they go out of business or their business strategy changes and they stop selling you their data.

In both cases, we encourage you to think critically about datasets that come from outside your organization. By definition, if some data is openly available—for free or for money—it means that everyone else can get it too. Often, projects based on external data sources will have a harder time on the market because they can easily be replicated by competitors. Some of the strongest AI projects are built on a foundation of proprietary datasets that are hard for others to reproduce.

To sum up, the walls of your organization (or the virtual ones of its data center) make a major difference in the data strategy of your AI project, as shown in figure 8.2. If data sits within the boundaries of your organization, you have a unique and highly valuable dataset you can use. Otherwise, you have to either get it for free or buy it. In both cases, take into account the liabilities that come with those options.

Of course, you can combine internal and external data. Actually, this is often a good idea. An example is our real estate house-price predictor: we could have used free government data on the income of each neighborhood to improve the model. You can even use data from OpenStreetMap or from Google Maps to check the presence of services and public transportation in various neighborhoods, adding another dimension to the house-price predictor.

For a project we built for a large organization, we used all kinds of data. The company had sales data from its stores, but we wanted to see correlations between sales performance and demographics, so we gathered free census data. The census data that was freely available was OK most of the time, but for some cities we needed a more fine-grained picture of the population. We then turned to specialized external providers and integrated their data as well. Notice that whereas sales data is updated

![](img/51e9508eebbc565953ed15bd4b5bec8f_192_0.png)

Figure 8.2 The digital or physical walls of your organization define the boundaries between types of data: core and ancillary data within your organization, and free or paid data outside of it.
