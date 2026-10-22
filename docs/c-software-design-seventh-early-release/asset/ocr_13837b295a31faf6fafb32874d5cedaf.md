segregate, or extract pieces of functionality:¹⁹

> *Systems that are broken up into small, well-named, understandable pieces enable faster work.*
> 
> —Michael Feathers

The intent behind separation of concerns is to better understand and manage complexity and thus design more modular software. This idea is probably as old as software itself and hence has been given many different names. For instance, the same idea is called *Orthogonality* by the Pragmatic Programmers.²⁰ They advise to separate orthogonal aspects of software. Tom DeMarco called it *Cohesion*:²¹

> *Cohesion is a measure of the strength of association of the elements inside a module. A highly cohesive module is a collection of statements and data items that should be treated as a whole because they are so closely related. Any attempt to divide them up would only result in increased coupling and decreased readability.*
> 
> —Tom DeMarco

In the *SOLID* principles, one of the most established sets of design principles, the idea is known as the *Single-Responsibility Principle (SRP)*.²²

> *A class should have only one reason to change.*
> 
> —Robert C. Martin

Although the concept is old and although it is commonly known under many names, many attempts to explain separation of concerns raise more questions than they give answers. This is particularly true for the *SRP*. The name of this design principle alone raises questions: what is a responsibility? And what is a *single* responsibility? A common attempt to clarify the vagueness about *SRP* is the following:

> *Everything should do just one thing.*
> 
> —Common Wisdom?

Unfortunately this explanation is hard to outdo in terms of vagueness. Just as the word *responsibility* does not carry a lot of meaning, *just one thing*