
Language was developed by humans as a means for structured, high conceptual bandwidth communications. Without language, human societies would not be functional. Over the years language has become richer as new concepts have emerged. Now that machines ‘get’ human language, we have a universal protocol for human-to-machine communication -- we can talk to them in our language, and they can respond in kind.

Interestingly, as a consequence of the invention of LLMs, we now also have a much richer and more robust machine-to-machine communication language in the form of our own natural language. Since machines understand English or German, Chinese or Hindi, we can have them communicate in one or more language, and this has the double benefit of being rich in abstractions and concepts, covering the entirety of humans-developed ontologies, and it has the added benefit that it is understood by us, which is helpful if we want to understand and follow the decision-making process.

So, out of the box, we have a universal protocol for inter-agent communications. Because of the flexibility of natural language to express intent, and the ability of LLM-based agents to map such intent to the manner by which their tools are called, software systems of the future will be much less brittle than the ones we have today: an agent responsible for a functionality takes care of mapping intent to the specific API calls, and so it will be much less of a hassle to change the API, or employ a different software with similar capabilities. This will give organizations more flexibility in upgrades or with third-party services.

If inter-agent communications and coordination are done in a manner that guarantees the encapsulation of responsibilities, then future enterprise applications will be much more robust, with new capabilities and functionality being introduced to a preexisting network of agents with little to no need for reengineering the system that is already there.

Given how a machine learning approach, using deep learning and transformer architectures, and training on language modeling, has resulted in such capable systems as LLMs, it is natural to think that we are back on track to create a single model that is generally intelligent. In other words, some people think that building ever larger LLMs, and training them on more and more data, will result in an LLM that is more intelligent than humans in every aspect of human intelligence. It seems, however, that for very different reasons, we are now back to considering agent-based systems as a next natural step for software (see [Figure I-2](#introduction01.html#ch00_figure_2_1771364788596659)).

![](assets/ch00_figure_2_1771364788596659.png)

Generative AI is pre-trained and so it is difficult to make it fundamentally change its world model online. Generative AI is typically trained on publicly available data, so, not having information in its training that is specific to a commercial problem domain, say proprietary data a company may be collecting and using, it is not generally useful out of the box for business applications.

Also, LLMs need to be given a system prompt in order to be useful. A system prompt is part of the LLMs input context that keeps being repeated with every distinct input, so as to remind the LLM who it is and what is expected of it. Through system prompts, LLMs can take different personas and operate differently. This means that, to play a specific role within a knowledge workflow, we need to give an LLM specific instructions, declare it to have specific expertise, and give it a specific job description and set of responsibilities.

The ability of machines to both understand highly complex human-designed concepts and execute specific instructions has led to resurgence in interest in agent-based AI, or agentic AI. Can we get machines to chart their own path towards executing on complex goals, and engineer the set of instructions to make that happen, and do so autonomously?

The challenge is altogether very different to that which the early AI pioneers faced in the nineties: back then, we were forced to simplify the problem domain because of lack of compute resources and model capabilities. Now, we still simplify the problem domain but that is because Generative AI models are too powerful and flexible, and we need to contain them and taskmaster them to specific roles within our organizational use-cases. Thus the advent of modern LLM-based Agents.

# Multi-Agent Systems

Many uses of Generative AI call for confining an LLM to a specific role. In figure 3, for example, we could choose to have the LLM respond to the query “What is the average weight of penguins on Biscoe?”. It may already have learned the answer from its general knowledge, but if it has not, it will try to guess, and the answer might not be reliable. But there is a more reliable alternative: if we have access to a database that we know to include reliable data, say on penguins, it is preferred that we tell the LLM about the schema of the database and the type of SQL used to query it, and have it translate our natural language inquiry into a call into the database.

LLMs are great at writing code, but they don’t have the facility to make the actual call and retrieve the data, so we need some helper code for that. Our code would pattern match this intent of the LLM and make the call to the database, retrieve the answer, and pass it on to the LLM to formulate it into a natural language response (see [Figure I-3](#introduction01.html#ch00_figure_3_1771364788596682)).

![](assets/ch00_figure_3_1771364788596682.png)

In fact, a popular use-case of Generative AI these days is Retrieval Augmented Generation (RAG). Many companies are seeking a ChatGPT-like interface to their proprietary document repositories, and this is implemented by summarizing the documents offline and storing them, along with the LLM’s representation of the summaries (known as embeddings), in a vector database. The embeddings are vectors capturing the meaning, or semantics, of the summaries. By storing them in a vector database, we can calculate numerical distances between the summaries, making it easier to search by meaning, rather than strict and rigid pattern matching of the words and sentences. This system can then respond to future natural language questions regarding the proprietary documents by mapping the requests to queries to the vector database, finding summaries that are closest in their meaning, to the query at hand.

Hence, you can see here too that RAG can be thought of as a multi-agent system, with an agent for offline summarization, and one tasked with responding to the user’s natural language queries (see [Figure I-4](#introduction01.html#ch00_figure_4_1771364788596701)).

![](assets/ch00_figure_4_1771364788596701.png)

But much more can be done. Any workflow requiring knowledge workers can be thought of as a multi-agent system. In [Figure I-5](#introduction01.html#ch00_figure_5_1771364788596719), for instance, we can have a planning agent come up with various plans, say for an upcoming vacation, an actuation agent with knowledge of APIs for internet services such as Expedia or Hotels.com, can then call the API and get actual numbers, say for airline price or hotel dates, into the plan. And a critic agent can check the results and decide whether they are plausible (e.g., is the connection time for a flight reasonable?) and send them back for refinement if they are not.

![](assets/ch00_figure_5_1771364788596719.png)
