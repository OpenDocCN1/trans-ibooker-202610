
![](img/b742a71dea5d5dd509847f82600a6ad1_68_0.png)

Figure 3-2. Enterprise-wide entity relationship diagram

As the saying goes, “jack of all trades, master of none.” Such models are supposed to be suitable for everything but eventually are effective for nothing. No matter what you do, you are always facing complexity: the complexity of filtering out extraneous details, the complexity of finding what you do need, and most importantly, the complexity of keeping the data in a consistent state.

Another solution would be to prefix the problematic term with a definition of the context: “marketing lead” and “sales lead.” That would allow the implementation of the two models in code. However, this approach has two main disadvantages. First, it induces cognitive load. When should each model be used? The closer the implementations of the conflicting models are, the easier it is to make a mistake. Second, the implementation of the model won’t be aligned with the ubiquitous language. No one would use the prefixes in conversations. People don’t need this extra information; they can rely on the conversation’s context.

Let’s turn to the domain-driven design pattern for tackling such scenarios: the bounded context pattern.

## What Is a Bounded Context?

The solution in domain-driven design is trivial: divide the ubiquitous language into multiple smaller languages, then assign each one to the explicit context in which it can be applied: its *bounded context*.

In the preceding example, we can identify two bounded contexts: marketing and sales. The term *lead* exists in both bounded contexts, as shown in Figure 3-3. As long as it bears a single meaning in each bounded context, each fine-grained ubiquitous language is consistent and follows the domain experts’ mental models.

![](img/b742a71dea5d5dd509847f82600a6ad1_69_0.png)

*Figure 3-3. Tackling inconsistencies in the ubiquitous language by splitting it into bounded contexts*

In a sense, terminology conflicts and implicit contexts are an inherent part of any decent-sized business. With the bounded context pattern, the contexts are modeled as an explicit and integral part of the business domain.

## Model Boundaries

As we discussed in the previous chapter, a model is not a copy of the real world but a construct that helps us make sense of a complex system. The problem it is supposed to solve is an inherent part of a model—its purpose. A model cannot exist without a boundary; it will expand to become a copy of the real world. That makes defining a model’s boundary—its bounded contexts—an intrinsic part of the modeling process.

Let’s go back to the example of maps as models. We saw that each map has its specific context—aerial, nautical, terrain, subway, and so on. A map is useful and consistent only within the scope of its specific purpose.

Just as a subway map is useless for nautical navigation, a ubiquitous language in one bounded context can be completely irrelevant to the scope of another bounded context. Bounded contexts define the applicability of a ubiquitous language and of the model it represents. They allow defining distinct models according to different problem domains. In other words, bounded contexts are the consistency boundaries of ubiquitous languages. A language’s terminology, principles, and business rules are only consistent inside its bounded context.

## Ubiquitous Language Refined

Bounded contexts allow us to complete the definition of a ubiquitous language. A ubiquitous language is not “ubiquitous” in the sense that it should be used and applied “ubiquitously” throughout the organization. A ubiquitous language is not universal.

Instead, a ubiquitous language is ubiquitous only in the boundaries of its bounded context. The language is focused on describing only the model that is encompassed by the bounded context. As a model cannot exist without a problem it is supposed to address, a ubiquitous language cannot be defined or used without an explicit context of its applicability.

## Scope of a Bounded Context

The example at the beginning of the chapter demonstrated an inherent boundary of the business domain. Different domain experts held conflicting mental models of the same business entity. To model the business domain, we had to divide the model and define a strict applicability context for each fine-grained model—its bounded context.

The consistency of the ubiquitous language only helps to identify the widest boundary of that language. It cannot be any larger, because then there will be inconsistent models and terminology. However, we can still further decompose the models into even smaller bounded contexts, as shown in Figure 3-4.

![](img/b742a71dea5d5dd509847f82600a6ad1_71_0.png)

Figure 3-4. Smaller bounded contexts

Defining the scope of a ubiquitous language—its bounded context—is a strategic design decision. Boundaries can be wide, following the business domain’s inherent contexts, or narrow, further dividing the business domain into smaller problem domains.

A bounded context’s size, by itself, is not a deciding factor. Models shouldn’t necessarily be big or small. Models need to be useful. The wider the boundary of the ubiquitous language is, the harder it is to keep it consistent. It may be beneficial to divide a large ubiquitous language into smaller, more manageable problem domains, but striving for small bounded contexts can backfire too. The smaller they are, the more integration overhead the design induces.

Hence, the decision for how big your bounded contexts should depend on the specific problem domain. Sometimes, using a wide boundary will be clearer, while at other times, decomposing it further will make more sense.

The reasons for extracting finer-grained bounded contexts out of a larger one include constituting new software engineering teams or addressing some of the system’s nonfunctional requirements; for example, when you need to separate the development lifecycles of some of the components originally residing in a single bounded context. Another common reason for extracting one functionality is the ability to scale it independently from the rest of the bounded context’s functionalities.

Therefore, keep your models useful and align the bounded contexts’ sizes with your business needs and organizational constraints. One thing to beware of is splitting a coherent functionality into multiple bounded contexts. Such division will hinder the ability to evolve each context independently. Instead, the same business requirements and changes will simultaneously affect the bounded contexts and require simultaneous deployment of the changes. To avoid such ineffective decomposition, use the rule of thumb we discussed in Chapter 1 to find subdomains: identify sets of coherent use cases that operate on the same data and avoid decomposing them into multiple bounded contexts.

We’ll discuss the topic of continuously optimizing the bounded contexts’ boundaries further in Chapters 8 and 10.

## Bounded Contexts Versus Subdomains

In Chapter 2, we saw that a business domain consists of multiple subdomains. So far in this chapter, we explored the notion of decomposing a business domain into a set of fine-grained problem domains or bounded contexts. At first, the two methods of decomposing business domains might seem redundant. However, that’s not the case. Let’s examine why we need both boundaries.

### Subdomains

To comprehend a company’s business strategy, we have to analyze its business domain. According to domain-driven design methodology, the analysis phase involves identifying the different subdomains (core, supporting, and generic). That’s how the organization works and plans its competitive strategy.

As you learned in Chapter 1, a subdomain resembles a set of interrelated use cases. The use cases are defined by the business domain and the system’s requirements. As software engineers, we do not define the requirements; that’s the responsibility of the business. Instead, we are analyzing the business domain to identify the subdomains.

### Bounded Contexts

Bounded contexts, on the other hand, are designed. Choosing models’ boundaries is a strategic design decision. We decide how to divide the business domain into smaller, manageable problem domains.

### The Interplay Between Subdomains and Bounded Contexts

Theoretically, though impractically, a single model could span the entire business domain. This strategy could work for a small system, as shown in Figure 3-5.
