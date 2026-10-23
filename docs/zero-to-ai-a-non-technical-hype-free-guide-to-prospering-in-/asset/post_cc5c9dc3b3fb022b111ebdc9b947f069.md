
From the point of view of development risk, the Build choice is the riskiest, because your team has full responsibility for the product. Luckily, they’re often not alone, and can rely on two important accelerators of progress: open source models and hosted infrastructure. Let’s see how each can help you build your project.

*Open source models* are freely available software (and, often, training data) that solves a specific problem. As long as its license is compatible with your intended use (for example, commercial use), your engineers can simply copy the code, customize it as needed, and use it in your product. If you’re not familiar with the open source world, you might be wondering why people would offer their work for free. Well, the two biggest contributors of open source models are large tech companies and academics. Tech providers like Google and Microsoft release cutting-edge products to strengthen their reputation as AI leaders and attract people to their turnkey products and ML platforms. Researchers in academia publish their software to increase awareness of their work, attracting funding and recognition. For more niche cases, you might not even find any open source code at all. Your best bet then is to get your researchers to dig up relevant scientific papers from the academic community. Often, the description of the model included in the paper is enough for your ML engineers to replicate it with little effort, and then you’re off to collecting training data.

Open source code is usually a great way to jump-start your development efforts. Your team will be able to start from a working solution and iterate to adapt to your specific problem. Compared to ML platforms, the ability to tweak the model enables the ML engineers in your team to debug problems and experiment with changes to improve its accuracy.

Generally speaking, if you build your own model, you’ll also need to provide and maintain the infrastructure for it, often by managing your own cloud resources. *Hosted infrastructure* frees your software engineers from this burden by letting you run your custom models in the cloud of a provider. As long as your model fits the provider’s technical requirements (for example, in terms of programming language), you can use their hardware and networks to run your models. This works because, as we explained in chapter 8, most of the magic in building AI lies in having the right data and creating a good model. The software to manage the execution has quickly been standardized and thus commoditized in the past few years.

Open source code and hosted infrastructure can do a lot to reduce the investment needed to build your own models, saving your team many man-years of work. Most importantly, they can also give you a confidence boost. At the beginning of the project, knowing that somebody else was able to crack the same problem will clear away many doubts about its feasibility. Toward the end of the project, having the option to offload much of the deployment work to a vendor makes the team run faster toward the finish line.

At the end of the day, which scenario (build, borrow, or buy) you land on depends on the specifics of each AI project you’ll work on. The next section will guide you through this decision.

## 9.2 Using the Lean Strategy

How can you choose which of the Buy, Borrow, and Build strategies is the best fit for your team and the project you’re working on? Is it even possible to find a single strategy that can fit a project at every stage? This section introduces our answer to this question: the *Lean AI Strategy*, a framework we developed to help you build your AI products incrementally.

The word *lean* comes from *lean manufacturing*, a philosophy born in the Japanese manufacturing industry (particularly in automotive companies like Toyota) that aims to minimize production waste without sacrificing efficiency. Eric Ries adapted the idea to the world of entrepreneurship in his landmark book *The Lean Startup* (Crown Business, 2011). Ries introduced a process to manage innovation that encourages testing market and technology assumptions through continuous experiments. The core insight behind this strategy is that even CEOs and founders have little information about what the marketplace truly values, so they’d better quickly build a first iteration of the product, see if customers like it, and then improve it based on their feedback.

The Lean AI Strategy has the same goal: it’s a process to incrementally implement AI projects while minimizing risk and resource waste (time, money, and team morale). Following the Lean AI Strategy, you’ll learn to choose the best implementation plan for a given project (build, buy, or borrow).

As you begin the project, the first step is understanding the technological risks associated with each AI development path. Each option has an associated complexity that comes with short-term risks, while giving you a degree of flexibility that is related to long-term liabilities. The relationships between these risks and flexibilities are represented in figure 9.5.

![](img/51e9508eebbc565953ed15bd4b5bec8f_213_0.png)

Figure 9.5  Points on the Build versus Buy spectrum stack up when it comes to risk and flexibility.

Now, imagine you’re just starting the implementation of an AI project. This is the moment where uncertainty is highest, mainly because of three factors:

- *Business risk*—Does my solution hold business value?
- *Workflow risk*—Can I successfully integrate my solution in the current business?
- *Technological risk*—Will the technology work well enough?

The methodologies introduced in chapter 7 are meant to minimize business and workflow risks, but nothing compares to real-world testing with real people. To put

your new product in front of real users, you'll often need to build at least some technology. The Lean Startup introduced the concept of minimum viable product (MVP): the smallest product you could possibly build while still delivering value to customers. In the world of AI projects, it is useful to think in terms of minimum viable AI (MVAI): the smallest and least risky technology that could possibly power your first MVP.

### 9.2.1 Starting from Buy solutions

The most important role of the Lean AI Strategy is getting you on the right path to build the MVAI. Given the high degree of uncertainty, it's a good idea to focus on a short-term strategy and defer long-term decisions to later, after the viability of the idea has been validated by customers. If you look at figure 9.6, this means starting from Buy solutions.

![](img/51e9508eebbc565953ed15bd4b5bec8f_214_0.png)

The technical vocabulary we taught you in part 1 is enough to start scouting for Buy solutions on Google. For instance, let's assume you run a food-delivery startup and want to sort food reviews into positive and negative ones. As you recall from chapter 5, this application is called sentiment analysis. A good query to search for Buy solutions is “Sentiment Analysis API,” which will return multiple results. In many cases, you'll find turnkey products from both large tech providers (such as Google, Amazon, or Microsoft) and smaller specialized companies. In the world of AI, you really should consider both. Large players have the advantage of scale, but small players might have the edge because their data collection and engineering efforts might be more closely aligned to the specific goals of your project.

If you find what you're looking for among the constantly growing offerings of AI providers, you're in luck. Adopting one of these is likely to be your best bet for developing the initial iteration of your AI project.

In other cases, you might not find a solution for the exact AI task your project needs. This isn't so uncommon; after all, providers invest resources into building only

those products that have a large potential market. If this is the case for your project, you have two options:

- Reframe your AI project so it fits into an existing Buy offering.
- Move up the technological risk ladder and look for Borrow solutions.

You should begin by considering option 1 first. Is there any way you can rephrase your AI task so you can use what's on the market without compromising your value proposition? For instance, let's assume you are a telco company and want to test a super-fast customer support service: users write a message, and the AI automatically routes the customer to the best call center agent; for instance, “My internet isn't working” would be a technical issue, and “I'd like to activate the new offer” a commercial claim. Aside from specialized AI companies with a full-blown product for your specific project, it's unlikely that you'll find a turnkey product for this exact task.

On the other hand, you'll find several very accurate sentiment analysis algorithms on the market today. Although these algorithms wouldn't solve 100% of the task, what about using them to route angry customers to customer support directly, and neutral customers to manual sorting by a human operator? Obviously, this won't be the final solution, but it allows you to test basic assumptions:

- *Business assumptions*—Are users willing to engage with customer support through an automatic system, or do they ignore it completely?
- *Workflow assumptions*—Is the new system a time-saver for operators?

If you can't find a way to scale down your problem as we did in this example, it's time to consider switching to Borrow solutions.

You also might consider moving to a Borrow solution when you've tried a Buy solution and it didn't perform well enough for the project. But first, you should reconsider what it means to be “good enough”: even if the AI isn't perfect and makes obvious mistakes from the point of view of a human, the project might still be a success for the organization. Even if that's not the case, take a second and pat yourself on the back. Thanks to the Lean AI Strategy and a focus on turnkey models, you've discovered important technical information about your project with little to no up-front investment in technology. Next, engage the ML engineers on your team to try to find out why exactly the Buy product doesn't work. Keep in mind that Buy products are the result of substantial engineering investments, and thus your team can hope to do better under only a single circumstance: lack of problem-specific training data (for example, cucumber classification, or a niche vocabulary in NLP tasks). For example, if you're finding that a Buy model has bad performance in translating documents from English to French, it's unlikely that you would be able to outdo Google and improve things by adding more training data. If you determine that the lack of task-specific training data is killing your project, Borrow models can be your way out of the problem.
