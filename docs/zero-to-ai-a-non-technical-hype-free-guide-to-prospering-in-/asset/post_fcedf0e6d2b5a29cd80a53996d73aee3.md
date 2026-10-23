
If high scalability and accuracy are two key drivers for your project, chances are that you’re looking at some very high ROI potential. Once you know this, you can factor in your readiness for each specific project to decide where to focus your energies in the next few months of your AI experimentation.

To clarify what this process looks like, let’s take another look at the projects that came out from the brainstorming session of our fictional real estate AI task force:

- *Room pictures classifier*—A feature that automatically sorts pictures based on the room
- *House-price predictor*—A feature that predicts the price at which a house will be sold
- *Broker assistant*—A kind of chatbot that can streamline a broker’s job or completely replace them

Let’s look at their *potential* first. The room pictures classifier has a high scale factor, as it allows people to upload and sort images in a split second instead of manually sorting them. Yet, the accuracy factor is low: we don’t care if our algorithm is better than humans at identifying bathrooms from kitchens, as long as it’s good enough.

The home-price predictor has a high scale factor, as right now guessing the sale price is a task reserved for experienced house brokers who are busy and can take days to make a prediction. This project can help clients get a quote in a second rather than days. The accuracy factor is also very high, as the better you are at predicting a house’s value, the more you can help clients with their financial planning.

The broker assistant has a high scale value as well, as it speeds up the response times for clients and spares brokers from answering simple questions, helping them focus on the activities they can do best. The accuracy factor isn’t very high, though, as brokers are already pretty good at answering clients’ questions.

Let’s now evaluate your *readiness*. You already have room pictures to build a room picture classifier. Also, the technology isn’t particularly hard to build, and it can even be bought from third-party providers. Testing it, on the other hand, requires some plumbing between the model and your current platform.

For the house-price predictor, you already have all the data you need. It’s also a *core business data* project with well-structured data, which doesn’t bring many technical challenges and doesn’t require crazy tech talents (you’ll get better at evaluating the complexity of a project in the next chapter). To test it, you also need to plug your website with your model (or some other clever hack—hold on for the next section).

The broker assistant is the most complicated one to build, because as you learned from chapter 5, text data is the trickiest of all the data types. You may have some data for it, and testing it can be done using the Facebook Messenger platform.

If in the Invention phase we came up with a list of projects, in the Prioritization phase we built a first educated guess of their potential based on their scale and accuracy factors, and then sorted them based on both their potential and your readiness to implement it. Now we know that the house-price predictor is the best project to focus

## Technological assumptions:

- We have the right data to build the model, and we can keep it up-to-date over time.
- The accuracy of the model is high enough that its estimates can be useful.

Because there’s often a way to work around technological roadblocks, we suggest that you test your business assumptions first. There’s no fix for a product that nobody wants.

There are ways to test business hypotheses before writing any line of code or collecting any byte of data. For instance, you could add a button labeled Get a Quote for your Home in 5 Minutes and route the requests to human brokers. We can call this validation strategy concierge AI: you are using people to test the attractiveness of a new feature that you’re intending to implement with AI. This is meant to be a temporary solution that won’t scale to millions of users, but you can use it to measure customer interest on a small sample of potential users.

An even simpler test you could run is to insert a fake button labeled Get an Instant Quote for your House that redirects users to a simple web page reading, “We’re sorry, this function is coming soon” or “Under maintenance.” You can try this trick with a few users, and count how many of them click your bait-button. Faking a function isn’t ideal, but it allows you to validate the potential of your AI projects in days (and with basically no budget) rather than months.

The output of such a test can be either positive or negative. If tons of users are clicking your Get a Quote in 5 Minutes button, it’s time to move forward to the content of the next chapter and start building that feature! In this case, the AI vision that our marketing manager laid down seems to be fruitful: there’s space to transform your real-estate platform into a full-blown financial consultant for real-estate investments.

It’s also possible that the results are negative. If not enough people show interest in the new functionality, maybe no one feels like they need lightning-quick home value estimates. In this case, don’t despair: your pre-product experiments allowed you to save tons of money and time building something no one wanted. This means that the AI vision of your marketing manager needs to be pivoted to something else, and you can start working on the next AI project in line; in our case, the room picture classifier.

Let’s assume that you test the room picture classifier, and it’s a resounding success. Its launch has taught you that customers do indeed place a lot of value on a pleasant listing experience. Armed with this additional knowledge, you can decide to focus on creating a real estate platform with the smoothest user experience. By the time you implement and release that feature, you’ll be free to jump into future projects with the confidence that all those AI projects have a place in the overall AI vision. Figure 7.5 shows how this flow of experiments and lessons can help you zero in on your AI vision.

Of course, this is just a simplified example. The real world is a messier place, with copious failed attempts and head scratching at every turn. However, the main point still stands: most successful AI-native companies didn’t start out with a complete strategy, but rather connected the dots until their AI mission emerged.

![](img/51e9508eebbc565953ed15bd4b5bec8f_173_0.png)

Figure 7.5 The evolution of the AI vision of our real estate platform example, and the different AI projects we discussed in reaching it.

If you want to use AI as a transformative technology, you’ll need to transform your mindset too. We encourage every organization, small or large, to adopt a mindset of data-driven experimentation and to invest in AI incrementally, learning from each project and crafting their vision along the way. The best way to do so is to focus on AI projects initially, and try to learn from each. If you’re careful to listen, the projects themselves will tell you where your AI vision should head, and they’ll be the building blocks of your vision.

Digging deeper into the methodologies of product experimentation is beyond the scope of this book, but you can find many resources online or in other books regarding the topic. Good places to start are The Lean Startup by Eric Ries (Crown Business, 2011) or anything by Steve Blank. What we want to leave you with is the idea that there’s no free lunch when it comes to developing new products, not even if you use a powerful technology like AI. If you haven’t tested your business assumptions and your idea is flawed, even the most powerful AI won’t save you.

Once your business assumptions have been tested, it’s time to transform your idea into an ML project. This is what we’ll do in the next sections and chapters.

## 7.5 Deconstructing an AI product

Let’s suppose our real estate company has validated the idea of building a bot to streamline the interaction between users and the platform. The idea is to make the platform feel like a flesh-and-blood broker, so you can ask questions like “What are the houses with three bedrooms in Manhattan’s Financial District?” and can receive a curated list of results, as a human broker would do. This is pretty much Apple’s Siri for real estate. Since you validated the idea, it’s time to go talk to some AI experts to build the product. Here’s how a hypothetical conversation would go in this case:

> Manager: Hey, John, we want to build a Siri for real estate. Is that feasible?
> 
> Data scientist (John): Sounds cool, but I need some more information. What’s the input data?
