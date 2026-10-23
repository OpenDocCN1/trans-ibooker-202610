
Notice that we applied this methodology to the case of an internet-based system that uses a freemium model (a free service and a paid upgrade), but it can be applied to any other case in which you have a group of customers performing one action and another group doing something else (or nothing). This scenario is common, and we’d like to encourage you to look for such situations and think about whether there’s space to build an ML classifier for it.

To give you some inspiration, here are some other cases where you can apply this methodology:

- You have a basic product and some upsells (accessories or additional services, which are common for telco companies). You can label customers with “has bought upsell X” or “hasn’t bought upsell X” and use their basic product usage to assess whether it may be worth proposing the upsell to your customer.
- You have a newsletter and want to optimize its open rates. Your labels are “has opened the newsletter” or “hasn’t opened the newsletter.” The features you use for the classifier may be the time you sent the email (day of the week, hour, and so forth) and some user-related features, and you may also tag emails by their content (for example, “informative,” “product news,” or “whitepaper”).
- You have a physical store with a fidelity card (to track which customer buys what). You can run marketing initiatives (newsletters again or also physical ads) and classify your users based on what brought them into your store and what didn’t.

As you can see, the method we just described of dividing users into two separate classes and building an ML classifier that can recognize the two is pretty flexible and can be used on a lot of problems. Pretty powerful, isn’t it?

## 3.4 Performing automated customer segmentation

In this chapter’s introduction, we referenced one of the key activities that marketers have to perform when developing a marketing plan: customer segmentation. Segmenting a market means dividing customers who share similar characteristics and behaviors into groups. The core idea behind this effort is that customers in the same group will be responsive to similar marketing actions. For example, a fashion retailer would likely benefit from having separate market segments for men versus women, and teenagers versus young adults versus professionals.

Segments can be more or less specific, and therefore more or less granular. Here are two examples:

- Broad segment—Young males between 20 and 25 years old
- Highly specific segment—Young males between 20 and 25 years old, studying in college, living in one of the top five largest US cities, and with a passion for first-person-shooter video games

Many marketers can intuitively perform this segmentation task in their brains as long as the amount of data is limited, both in terms of examples (number of customers) and features. This usually produces generic customer segments like the first one, which can be limiting considering the amount of variation that exists among these groups. A marketer could attempt to define a more specific segment like the second one, but how do they come up with it? Here are the questions that could be raised during a typical brainstorming session:

- Is it a good idea to use the 20- to 25-year-old threshold, or is it better to use 20 to 28?
- Are we sure that the college students living in large cities are fundamentally different from the ones living in smaller ones? Can’t we put all of them into a single cluster?
- Is there a fundamental difference between males and females? Do we really need to create two segments, or is this just a cliché?

Answering these questions can be done in three ways:

- Go by your gut feeling. We’re not in 1980, so don’t do that.
- Look at the data, and use the marketer’s instinct to interpret it. This is better than a gut feeling, but marketers will likely project their biases into their analysis and see what they want to see. So avoid this as well.
- Let AI come up with customer segments by itself, keeping a marketer in the loop to use their creativity and context knowledge.

Option 3 is most likely to outperform the others. Let’s see why and how.

### 3.4.1 Unsupervised learning (or clustering)

Let’s look at the problem we just described:

- What we start with: a pool of customers, with a bunch of elements that characterize them (age, location, interests, and so forth)
- What we want: a certain number of segments we can use to divide our customers

You can imagine this problem as having a bunch of customers and needing to place each one into a bucket, which we’ll call a cluster (figure 3.5).

![](img/51e9508eebbc565953ed15bd4b5bec8f_67_0.png)

Figure 3.5 A clustering algorithm divides a group of uniform customers into three clusters.

The customers’ characteristics that we’ll use resemble what we’ve been calling features in previous chapters, so you may think that we’re dealing with the same kind of task and can use the same tools we’ve already described. But the devil is in the details: because we don’t know in advance the groups we want to define, we don’t know which labels to apply. So far in the book, we’ve used a subset of ML called supervised learning. The typical recipe for a supervised learning task is the following:

1. We have data on a group of customers characterized by certain features.
2. These customers also have a label: a target value we’re interested in predicting (for example, whether they churned or not).
3. The supervised learning algorithm goes through the customers’ data, and learns a general mapping between features and labels.

In our new scenario, point 2 is missing: we don’t have a label attached to each user. This is what we want our new algorithm to find. Therefore, this is what our new task looks like:

1. As before, we have data on a bunch of customers characterized by certain features.
2. We want to divide customers into a certain number of segments (clusters)—let’s say three of them.
3. We run some kind of ML algorithm that, looking at the data, determines the best clusters we can come up with and splits the users into them.

This new kind of ML algorithm is called *clustering*, or *unsupervised learning*. Unsupervised learning is another form of ML in which an algorithm is fed with a set of unlabeled examples (just a set of parameters) and is asked to divide the examples into groups that share some similarity. In this sense, unsupervised learning algorithms use the concept of similarity to overcome the lack of a previously defined label, dividing the examples they’re fed into groups in an autonomous manner. This is the core difference between supervised and unsupervised learning: supervised learning algorithms learn a mapping between a set of features and labels, whereas unsupervised algorithms just look at labels and group data points based in clusters that share a certain similarity, as shown in figure 3.6.

technical people, and with an open mindset that can accept the inputs that are given to you by them.

A good way to visualize clustering is to picture how stars are scattered over the night sky. Our brain intuitively groups neighboring stars and assigns them zodiac signs. Most real-world applications are a bit more complex, for three main reasons:

- Sometimes, data points are distributed homogeneously, making it difficult to decide how many clusters to consider, let alone how to divide customers.
- As humans, we easily perform segmentation on limited dimensions, but we struggle when the number of dimensions increases. Think back to the zodiac sign examples: we see the sky as a two-dimensional canvas; performing the same task in a three-dimensional space would be much harder. In four dimensions, it would be impossible. What about with 20 dimensions of customer information?
- From a business standpoint, segmentation is not an idle exercise, but is most useful only when the various segments can be linked to a business outcome, often in terms of customer lifetime value, price sensitivity, or channel preference.

In the next section, we’ll address these issues through an example. For now, let’s dig a bit deeper into the nuts and bolts of clustering. One of the first important decisions to make when tackling a clustering problem is to decide which features (or dimensions) to use for clustering. In our trivial example of a night sky, the choice of dimensions is obvious: the horizontal and vertical position of each star. Real-world applications may be way more complex, though.

### 3.4.2 *Unsupervised learning for customer segmentation*

This section will give you more details on unsupervised learning and shed some light on the inner workings of these algorithms. The core concepts you’ve learned so far are enough to envision new applications for unsupervised learning within your organization. If that’s enough for your goals, feel free to skip this section. If you want to know more about how to use these techniques in practice, keep reading.

Let’s use the example of an e-commerce website that sells shoes and has access to the purchase history of its customers. Each data point represents a purchase or a return, and contains information about the shoe such as price, brand, size, color, the date and time of the transaction, and whether it was bought together with other items. We could decide to use all these features for the segmentation or to limit our analysis to a subset. For example, looking at the colors of all the shoes a customer bought might help us better understand that customer’s taste, or looking at the time of the day for those purchases may provide suggestions on the best time of the day to propose discounts.

We could even extrapolate parameters such as the average amount spent per purchase and the number of shoes bought in a month. These two pieces of information together would likely help the clustering algorithm find a natural distinction between high-frequency/low-value customers and low-frequency/high-value ones.
