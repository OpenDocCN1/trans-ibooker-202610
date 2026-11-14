
Let's focus firstly on the concepts present in the code. Start with the list of concepts and decide if you can explain in your own words what the domain concepts mean.

If you can't, reading more of the code might not be the best next step. Instead, you might want to start by gathering more information about the concepts from other sources by asking teammates or other contributors or reading comments and documentation.

It can be especially valuable to also investigate whether you suspect that synonyms are used in the code. While writing this exercise, I realized that in the Hedy code base we sometimes say `parse tree`, but also use `tree` in other places. While that might be very clear to the code team, using synonyms can be confusing for newcomers. And using synonyms can even impact experienced developers negatively, for example by hampering searchability, since looking for "parse tree" will not reveal locations of "tree" in the code.

# After reading

## Exercise 3.5: Describe concepts

Now is a good time to reexamine the concepts that you have gathered from the names in Exercise 3.2. Do you understand enough about the domain to describe the meaning of each concept? If not, describe this in the meaning column so you can go back to this list later when you have gathered more information.

| Concept | Meaning |
| --- | --- |
| `Bracket` | One bracket, can be unexpected if a closing bracket |
| `Bracket Pair` | Two brackets is code belonging together, should get the same color in the editor |
| … |  |
|  |  |
|  |  |

## Enrich domain understanding

Once you have investigated the code and made an effort to understand the domain concepts from the code itself, it can be very valuable to enrich your knowledge in two ways.

Firstly, you can direct your attention outside of the codebase. Depending on your context, this might be a deep dive into the documentation trying to obtain more information about the concepts that you have found in an open source project, or a session with an experienced colleague within a company. In both cases, you are looking to gather more depth in your understanding of the concepts that you have just learned about. A great exercise is to double check your findings from exercises 3.2 and 3.4. Can you verify that the meaning and synonyms from exercise 3.6a are correct, and that the datatype from exercise 3.6b are correct?

Secondly, after expanding your knowledge of all domain concepts with information outside of the code base, a second way to deepen understanding is to reread the code. Now is a great moment to let go of the small focus on the names in the code, and read the code as a whole again. Because your knowledge is now expanded, you will certainly pick up on new things in the code that did not occur to you before.

## Expand documentation

Some of the information about domain concepts and behaviour can serve as excellent documentation to add to a readme file. In particular, you have explored a lot of important concepts in the code, and these might not live somewhere in a documentation file yet. In a sense, you might have been starting to document the `Ubiquitous Language` of the project.

### Ubiquitous Language

Eric Evans coined the term `Ubiquitous Language` in his Domain Driven Design book. With the ubiquitous language, Evan denotes a language that developers and users of software systems share. Often this language only partially emerges naturally and parts of it will need to be deliberately designed. Evans states that the `Ubiquitous Language` should be based on the Domain Model used in the software since natural language concepts tend to be imprecise.

The information that you have gathered so far could be converted into documentation. The lists of concepts that you created in exercises 3.6a and 3.6b can serve as starting points for a list of domain concepts. From there, you can expand, for example by:

*   Ensuring that all abbreviations or acronyms in variable or function names are either replaced by full words, or if people prefer keeping them, documented
*   Describing domain objects that felt similar to you but turned out to be different. If they are not exactly the same but there are reasons that they look similar, such as similar names or similar behaviour, other people might be confused too. Documenting how these concepts differ subtly can be very useful
*   Discussing these lists with users. Do they recognize and understand all concepts? What concepts are missing and are they maybe missing or represented in other ways?
*   If you work a context where DDD is practiced and a `Ubiquitous Language` is used, you can check whether the list of your concepts is similar to the documentation of the `Ubiquitous Language` that already exists.

Furthermore, you will likely find some names that do not exactly describe the functionality of code, such as `analyze()`, `convert()`, or `process()`. While reading the code, you might have found a deeper understanding of those words, based on examining the call trace of the function, variable names in the function or comments. You might be able to commit to the documentation, specifically the `docstrings` of these functions to aid future readers.

## Discuss code quality

While examining the code from the domain perspective, you might find parts of the code that have code smells, specifically linguistic code smells, which are code smells relating to the naming of objects, functions, methods or variables. Your domain findings can serve as a great conversation starter with your team to talk about naming quality.

**Similar objects**

Your domain analysis might point to objects that seem very similar. Apart from documenting the differences, as explained above, you might also want to dive deeper into the reasons that the code is like it is, for example by discussing:

*   Are these domain concepts truly synonyms or are they different, and if so, how?
*   Are there historical reasons for the similarities? Sometimes when a new product or process is introduced, it comes with its own objects since the author might not be aware of places in the code where it could fit.

**Linguistic code smells**

Chapter 2 discussed two different code smells: structural smells as described by Fowler, and linguistic code smells as defined by Arnaoudova. Since this chapter focuses on domain concepts, it makes sense to evaluate the quality of the names in terms of smells. You can focus on specific types of linguistic smells, which Arnaudova distinguished between in her work, three for functions/methods and three for identifiers.

*   Methods that do more than they say
*   Methods that say more than they do
*   Methods that do the opposite of what they say
*   Identifiers whose names say that they contain more than what the entity contains
*   Identifiers whose names say that they contain less than what the entity contains
*   Identifiers whose names say the opposite of what the entity contains
