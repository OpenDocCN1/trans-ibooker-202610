
goal is true, I don’t mean to imply that this is easy work. The truth is that bringing data governance to your organization will take time, patience, and the willingness to start small. Trying to do too much, too fast could cause the whole house to collapse.

So, your homework for this chapter is to pick one initial project for your data governance council to work on after you’ve completed all the steps in this chapter. To gain sponsorship buy-in, make sure this project aligns with their priorities. This will give you the chance to build a scalable process for governing data projects, get stewards involved at a manageable pace, and show why governance is the most effective way to manage big data.

When choosing your data governance council’s first project, consider the current appetite for data governance. Even if your organization owns a lot of data, don’t assume that senior leadership sees the need for governance. Even if they do know that it’s needed, the truth is that they might not be ready or willing to lead the culture change required. All isn’t lost if this sounds like your business, but that does make it even more important to take an incremental approach.

Recall that your job is to manage data at scale. You’re expected to lead the tools, structure, and strategy required for your colleagues to complete successful data projects. If you can prove that data governance is the most effective way to smash this goal, you will gain more buy-in from the C-suite and prospective data stewards. The best way to gain buy-in for data governance is to start small and lead one specific data project to success. You’ll learn how to do this in the next chapter.

## CHAPTER 4

## Write Your Data Roadmap

In early 2000, Reed Hastings wanted to sell. As Hastings and his co-founder Marc Randolph sat in Blockbuster’s Dallas office, Hastings whispered that since the video rental giant was a thousand times their size, selling their startup for $50 million was in their best interest. When Blockbuster’s CEO John Antioco entered the room, Hastings proposed that if Blockbuster bought their business, they could run Blockbuster.com as its online video rental arm. But when he heard how much the acquisition would cost, Antioco declined.

You know what happened next. That startup, Netflix, went public two years later and had a $238.89 billion market cap 20 years later. Blockbuster, once the world’s premier video rental service, filed for bankruptcy in 2010 before shutting all corporate-owned stores four years later. Its highest market cap? $3 billion.

As the video rental industry market’s former leader, Blockbuster sat on a mountain of data that it could have used to keep growing. Had leadership seen the growth in demand for online services, they had the resources to crush competitors. Instead, they had to open their own online video rental store years after Netflix started cornering that market. By then, it was too late for them to build the data-first culture that made Netflix the new leader.

When it comes to data, most organizations are more like Blockbuster than they’d care to admit. Gartner research classified 87 percent of organizations as having low maturity in BI and analytics techniques.¹ The same research found that organizations with low maturity rarely share data between IT and

1. https://www.gartner.com/en/newsroom/press-releases/2018-12-06-gartner-data-shows-87-percent-of-organizations-have-low-bi-and-analytics-maturity#:~:text=More%20than%2087%20percent%20of,a%20survey%20by%20Gartner%2C%20Inc

business users. They also don’t link data to clear business outcomes, and bottlenecks in the central IT team often impede data model preparation.

Whether your business wants to leverage customer data to make tailored recommendations or build new machine learning models, you can’t achieve any data-based goal without a plan to get there. This chapter will share how to use a data governance roadmap as the blueprint for your data-based projects and products.

You already know how essential roadmaps are. They put your product strategy to paper, defining every product against bigger goals and initiatives like the mission statements you wrote in Chapter 1 Clarify Your Company’s Mission Statement on page 2. Effective roadmaps paint a clear picture of your product’s direction, mapping each feature per sprint up to key performance indicators (KPIs).

The most advanced product roadmaps invite new ideas. I once worked for a roadmap software company which offered an ideas portal. This portal helped users collect feedback for new features, then prioritize them against company goals.

Now, apply that mindset to your data governance strategy. Thus far, you’ve written a mission statement for how your business will use data. You’ve also found data stewards who will bring that mission statement to life, and formed a data governance council to ensure that all stewards work together. In this chapter, you’ll learn how to write and manage all the content on a data governance roadmap to serve as a living, single source of truth for your data strategy. To begin, break this work down into distinct phases.

### Assess Your Data’s Current State

You can’t make progress if you don’t know where you stand. It’s why the first step in writing your data roadmap might be the most painful: you’ll need to do a complete, honest audit of how your business uses data today. Most businesses overestimate their aptitude with data, and that (lack of) aptitude affects all the ways your business uses data. Your audit will be most effective if you start with some key considerations.

To start, there’s compliance with data privacy laws. Many of these laws are relatively new, from the General Data Protection Regulation (GDPR) in Europe to the California Consumer Privacy Act (CCPA) in California. Governments enacted them to protect consumer data from misuse by businesses, like selling that data to third parties. Regardless of where your business headquarters are, it’s likely that these laws affect you.

For instance, GDPR limits business's abilities to collect key data about European Union (EU) citizens, such as age, location, and socioeconomic status. Under GDPR, businesses that want to use these details must inform users of their intent to do so, and how they plan to collect that data. They must also give each person the right to explicitly opt into data profiling, and they can't subject customers to automated profiling. If your business has personally identifiable data about any EU citizens, your business must comply with GDPR even if it's based elsewhere.

Compliance is one factor, but it doesn't end there. You must also understand data in the context of your data architecture. Let's revisit the case study of redesigning a database from What Are Data Stewards? on page 14. Recall that in this case study, you must redesign the database taxonomy because it's too tough for users to find content in the database as-is. To achieve this goal, your team's data architect asks you where a certain dataset lives. She needs this information to build a data model that accurately shows how data flows from one system to the next before that data appears on the database front-end.

If she asked for this dataset, would you know where it's stored? If you don't know offhand, do you have it documented somewhere so she can reference it? Experience taught me that for many businesses, the answer is a dual no. They don't have a clue where all their data lives, nor do they have it documented. This makes it impossible to improve your data infrastructure.

Last but not least, data quality is a concern. Do the same datasets live in two separate locations? Do your colleagues automate any of their data management, or is it all manual? If your business publishes proprietary data, who cross-checks it for accuracy?

To complete an accurate data audit, you'll need to find and write down answers to several questions about data compliance, data architecture, and data quality. It helps to break them down per section, like this.

Data Compliance:
- Whom do you collect data on?
- Which type of data do you collect?
- Where do these people live?
- Which (if any) data privacy laws protect people in the geographic regions where they live?

Data Architecture:
- Where does your organization store data?
- Do these databases integrate with each other? If so, how?
