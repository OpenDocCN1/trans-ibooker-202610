
In our apartment, we do not have a standard entry into the kitchen. The cardboard was cut precisely to the size of the fridge’s width and depth. The problem it solves is checking whether the refrigerator can fit through the kitchen door (see Figure 3-10).

![](img/b742a71dea5d5dd509847f82600a6ad1_81_0.png)

Figure 3-10. The cardboard model in the kitchen doorway

Despite the cardboard not looking anything like the fridge, it proved extremely useful when we had to decide whether to buy this model or opt for a smaller one. Again, all models are wrong, but some are useful. Building a 3D model of the fridge would definitely be a fun project. But would it solve the problem any more efficiently than the cardboard? No. If the cardboard fits, the 3D model would fit as well, and vice versa. In software engineering terms, building a 3D model of the fridge would be gross overengineering.

But what about the refrigerator’s height? What if the base fits, but it’s too tall to fit in the doorway? Would that justify gluing together a 3D model of the fridge? No. The problem can be solved much more quickly and easily by using a simple tape measure to check the doorway’s height. What is a tape measure in this case? Another simple model.

So, we ended up with two models of the same fridge. Using two models, each optimized for its specific task, reflects the DDD approach to modeling business domains. Each model has its strict bounded context: the cardboard verifying that the refrigerator’s base can make it through the kitchen’s entry, and the tape measure verifying that it’s not too tall. *A model should omit the extraneous information irrelevant to the task at hand.* Also, there’s no need to design a complex jack-of-all-trades model if multiple, much simpler models can effectively solve each problem individually.

A few days after I published [this story on Twitter](https://twitter.com), I received a reply saying that instead of fiddling with cardboard, I could have just used a mobile phone with a LiDAR scanner and an augmented reality (AR) application. Let’s analyze this suggestion from the domain-driven design perspective.

The author of the comment says this is a problem that others have already solved, and the solution is readily available. Needless to say, both the scanning technology and the AR application are complex. In DDD lingo, that makes the problem of checking whether the refrigerator will fit through the doorway a generic subdomain.

## Conclusion

Whenever we stumble upon an inherent conflict in the domain experts’ mental models, we have to decompose the ubiquitous language into multiple bounded contexts. A ubiquitous language should be consistent within the scope of its bounded context. However, across bounded contexts, the same terms can have different meanings.

While subdomains are discovered, bounded contexts are designed. The division of the domain into bounded contexts is a strategic design decision.

A bounded context and its ubiquitous language can be implemented and maintained by one team. No two teams can share the work on the same bounded context. However, one team can work on multiple bounded contexts.

Bounded contexts decompose a system into physical components—services, subsystems, and so on. Each bounded context’s lifecycle is decoupled from the rest. Each bounded context can evolve independently from the rest of the system. However, the bounded contexts have to work together to form a system. Some of the changes will inadvertently affect another bounded context. In the next chapter, we’ll talk about the different patterns for integrating bounded contexts that can be used to protect them from cascading changes.

## Exercises

- 1. What is the difference between subdomains and bounded contexts?
- a. Subdomains are designed, while bounded contexts are discovered.
- b. Bounded contexts are designed, while subdomains are discovered.
- c. Bounded contexts and subdomains are essentially the same.
- d. None of the above is true.
- 2. A bounded context is a boundary of:
- a. A model
- b. A lifecycle
- c. Ownership
- d. All of the above

3. Which of the following is true regarding the size of a bounded context?

The smaller the bounded context is, the more flexible the system is.

- a. Bounded contexts should always be aligned with the boundaries of subdomains.
- b. The wider the bounded context is, the better.
- c. It depends.

4. Which of the following is true regarding team ownership of a bounded context?

- a. Multiple teams can work on the same bounded context.
- b. A single team can own multiple bounded contexts.
- c. A bounded context can be owned by one team only.
- d. B and C are correct.

5. Review the example of the WolfDesk company in the Preface and try to identify functionalities of the system that may require different models of a support ticket.

6. Try to find examples of real-life bounded contexts, in addition to those described in this chapter.

charge of both software engineering and business development. As a result, you have the ability to affect both the software design (bounded contexts) and the business strategy (subdomains). Therefore, in the (bounded) context of our discussion here, we are focusing only on software engineering.

2 Bredemeyer Consulting, “What Is Software Architecture.” Retrieved September 22, 2021, https://www.bredemeyer.com/who.htm

## Chapter 4. Integrating Bounded Contexts

Not only does the bounded context pattern protect the consistency of a ubiquitous language, it also enables modeling. You cannot build a model without specifying its purpose—its boundary. The boundary divides the responsibility of languages. A language in one bounded context can model the business domain to solve a particular problem. Another bounded context can represent the same business entities but model them to solve a different problem.

Moreover, models in different bounded contexts can be evolved and implemented independently. That said, bounded contexts themselves are not independent. Just as a system cannot be built out of independent components—the components have to interact with one another to achieve the system’s overarching goals—so, too, do the implementations in bounded contexts. Although they can evolve independently, they have to integrate with one another. As a result, there will always be touchpoints between bounded contexts. These are called *contracts*.

The need for contracts results from differences in bounded contexts’ models and languages. Since each contract affects more than one party, they need to be defined and coordinated. Also, by definition, two bounded contexts are using different ubiquitous languages. Which language will be used for integration purposes? These integration concerns should be evaluated and addressed by the solution’s design.

In this chapter, you will learn about domain-driven design patterns for defining relationships and integrations between bounded contexts. These patterns are driven by the nature of collaboration between teams working on bounded contexts. We will divide the patterns into three groups, each representing a type of team collaboration: cooperation, customer–supplier, and separate ways.

### Cooperation

Cooperation patterns relate to bounded contexts implemented by teams with well-established communication.

In the simplest case, these are bounded contexts implemented by a single team. This also applies to teams with dependent goals, where one team’s success depends on the success of the other, and vice versa. Again, the main criterion here is the quality of the teams’ communication and collaboration.

Let’s look at two DDD patterns suitable for cooperating teams: the partnership and shared kernel patterns.

### Partnership

In the partnership model, the integration between bounded contexts is coordinated in an ad hoc manner. One team can notify a second team about a change in the API, and the second team will cooperate and adapt—no drama or conflicts (see Figure 4-1).

![](img/b742a71dea5d5dd509847f82600a6ad1_87_0.png)

Figure 4-1. The partnership model

The coordination of integration here is two-way. No one team dictates the language that is used for defining the contracts. The teams can work out the differences and choose the most appropriate solution. Also, both sides cooperate in solving any integration issues
