
# Scalability and Flexibility in RL Deployment

## Scalability

*Scalability* is particularly important to both machine learning and RL, arguably more so than typical software, because they need so much horsepower to do their job. If you consider all software ever written, the vast majority of those applications will probably run on the resources provided by a standard laptop. Only rarely do you need to build something that is as scalable as a global messaging or banking system. But in machine learning and RL, nearly every single product ever produced needs far more than what a laptop can provide. I posit that the average computational requirement for an RL product is several orders of magnitude greater than a software product. I am being intentionally vague, because I don't have any hard facts to support this. But I think my hypothesis is sound and this is why scalability is an important factor when deploying RL.

## Flexibility

I think *flexibility* is one of the most underrated best practices. In industry, reliable software was the theme of the first decade in the 21st century, with software architecture, test-driven development, and continuous integration being hot topics at conferences around the world. In the second decade, scalability was the golden child and resulted in important changes like cloud adoption and the subsequent need for orchestration. I think the third decade (I'm ignoring specific themes like artificial intelligence and RL and machine learning) will focus on flexibility, purely because I see industry trending toward hyper-responsibility, where the most valuable engineers are defined by their ability to fulfill all of the roles expected in an enterprise environment, from business development through to operations. The field of DevOps can only ever expand; it can never shrink back to two separate roles.

A similar argument can be made about the pace of change today. Engineers are continuously creating new technologies and it's hard to predict which will withstand the test of time. But I find that when tools attempt to enforce a fixed, inflexible, or proprietary way of working, they quickly become cumbersome to work with and are quickly outdated. No, flexibility is the only way to future-proof systems, processes, and solutions.

## Hierarchy of Needs

Maslow's hierarchy of needs was an early psychological model suggesting human necessities depicted as a hierarchical model; in order to have safety you need access to air and water, in order to have friendship you need to be safe, and so on. RL has a similar hierarchy of needs.

To have a viable RL problem then you need an environment in which you can observe, act, and receive a reward. You need the computational ability to be able to decide upon actions given an observation. These things are required and if you don't have them, then you can't do RL.

Above that comes things that are valuable, but not strictly necessary. For example, pipelines to transform data in a unified, reliable way will prevent data errors. Being able to specify exactly what is running in production—provenance—is operationally useful.

The layer above this represents an optimal state, one that is probably more conceptual than practical in many cases, because tackling other problems or developing new features might be more valuable in the short term. Here you might consider fully automating the end-to-end process of whatever it is you are doing, or incorporating a sophisticated feedback mechanism to allow your product to automatically improve itself.

[Table 10-1](#ch10.html#tab_hierarcy_needs) presents some example implementation and deployment challenges that you might face. I want to stress that these have a range of solutions with variations in the amount of automation. But the value of each level of automation depends on your or your stakeholder's assessment of other pressing needs. Just because there is a more sophisticated solution that doesn't mean what you have isn't good enough already. Get the basics right first.

**Table 10-1.** Example hierarchy of needs for different implementation challenges: the bottom row represents basic requirements, the middle row represents some amount of automation, the top row represents the state-of-the-art

| | Infrastructure | State representation | Environment | Learning | Algorithm improvement |
|---|---|---|---|---|---|
| **Optimal** | Self-healing | Continuously learning | Combination | Meta | Automated |
| **Valuable** | Automated deployment | Model-driven dimensionality reduction | Real life | Offline | Brute-force selection |
| **Required** | Manual deployment | Fixed mapping | Simulation | Online | Manual |

## Architecture

There hasn't been a huge amount of research into generic RL architectures and I can think of a few reasons to explain this. The first is that RL still isn't universally accepted as a useful tool for industrial problems, yet. Hopefully this book will start to change that. This means that there isn't enough combined experience of designing these kinds of systems and therefore it has not been disseminated to a wider audience. Secondly, researchers, especially academic researchers, tend to be more focused on groundbreaking research, so you will find few papers on the subject. Third, I don't think there is a one-size-fits-all approach. Compare the differences between single-agent and multi-agent RL, for example. And finally, the deployment architecture is mostly a software engineering concern. Of course there are RL-specific needs, but I think that typical software architecture best practices fit well.

With that said, there is some literature available from the domain of contextual bandits. It's not generally applicable, but because industry has been quicker to adopt this technique, many lessons have already been learned. Two of the biggest problems that lead to architectural impacts are the fact that agents can only observe partial feedback and that rewards are often delayed or at worst, sparse[^29]:

- Machine learning is considered to be simpler, because feedback, or ground truth, is complete. The label represents the single correct answer (or answers if multiple are correct). RL only observes the result of a single action; nothing is learned from the unexplored actions (which are possibly infinite).
- Rewards observed by the agent are often delayed, where the delay ranges from millisecond scales in bidding contests to days when people are involved. Even then, the reward may be sparse because the sequence of actions doesn't lead to a real reward for some time, like a purchase.

Some problems have a relationship with prior work in machine learning, but are accentuated by RL. For example, online learning, or continuous learning, is important in some machine learning disciplines, but RL takes this to a whole new level, since decisions have wide reaching consequences and policies are nonstationary. Even if you are incorporating batch RL, it is important to have a low-latency learning loop. Reproducibility becomes a big problem, too, since both the policy and the environment are constantly shifting.

All of these facets lead to the following range of architectural abstractions that might be useful in your problem:

**Serving interface**

: Like a personalized shopping assistant, one abstraction should contain the ability to serve action suggestions and embed exploration. This should implement the Markov decision process interface and communicate with the wider system or environment. This is the both the entry and exit point of the system and presents a great location to decouple RL from wider deployments.

**Log**

:
