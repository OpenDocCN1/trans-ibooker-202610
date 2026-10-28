15.9 Quantum machine learning ......................................................... 618

15.10 Summary ......................................................... 619

# 15.1 What is machine learning?

Consider my complete product browsing and purchase history with an online retailer. This data represents my experience of buying from the seller and their experience selling to me. How can the data and my new purchases help the retailer sell me additional products and services?

Machine learning is a large set of techniques where computer algorithms learn from existing data rather than having hardcoded decisions built into them. Such an algorithm improves its accuracy as it receives new data. It might also get better when humans or processes intervene to judge the correctness and quality of previous results.

In *unsupervised learning*, an algorithm looks for patterns in the data that may help users gain insight. An unsupervised algorithm may help the retailer see that I have increased interests in cooking, travel, or home repair during certain seasons. The seller can then preemptively give me product recommendations several weeks before I would typically purchase them.

In *supervised learning*, the data also has among its features the “answer” to a question we might ask about it. Suppose the online retailer also has a travel bureau. I bought recommended books and clothes after I purchased the trip. In that case, the retailer can label the book and clothes data “purchased for travel.” Given new clothing purchases, the retailer’s machine learning algorithm might strongly suggest that I am looking to take a vacation and offer possible flights and hotels.

In *reinforcement learning*, the algorithm adds some kind of reward or penalty based on the outcome. If I purchase and then return a product, the algorithm might insert a negative score as a feature in the data associated with the deduced result. This feature could help the algorithm make a better future choice between two possible actions. Perhaps I am taking a trip, and I want to buy new shirts. If I purchase and then return some of them, the retailer can update the data to note that I don’t like long-tailed shirts with buttons when going to a warm climate.

Another way to get reinforcement data is to ask the user directly. If the retailer website shows you a product and then asks, “Do you like this recommendation?” the seller can use a “yes” or “no” response to tune its recommendation algorithm. Of course, if you purchase the product, that should produce positive reinforcement.