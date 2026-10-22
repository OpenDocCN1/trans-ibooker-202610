In “Guideline 7: Understand the Similarities between Base Classes and Concepts”, we will compare the two most commonly used abstractions: base classes and concepts. You will understand, that from a semantical point of view, both approaches are very similar since both are able to express expected behavior.

In “Guideline 8: Understand the Semantic Requirements of Overload Sets”, I will extend the discussion about semantic requirements and talk about a third kind of abstraction: function overloading. You will understand, that all functions being part of an overload set, also have an expected behavior and thus also have to adhere to the LSP.

In “Guideline 9: Pay Attention to the Ownership of Abstractions”, I will focus on the architectural meaning of abstractions. I will explain what an architecture is, and what we expect from the high and low levels of an architecture. I will also show you that from an architectural point of view it is not enough to just introduce an abstraction to resolve dependencies. To explain this, I will introduce the Dependency Inversion Principle (DIP), a vital advice on how to build an architecture by means of abstractions.

In “Guideline 10: Consider Creating an Architectural Document”, we will sit together and talk about the benefits of an architectural document. And hopefully this little chat will act as an incentive to create one in case this was not on your radar before.

# Guideline 6: Adhere to the Expected Behavior of Abstractions

One of the key aspects of decoupling software, and thus one of the key aspects of software design, is the introduction of abstractions. For that reason you would expect that this is a relatively straightforward, easy thing to do. Unfortunately, as it turns out, building abstractions is difficult.

In order to demonstrate what I mean, let us take a look at an example. I have selected the classic example for that purpose. Chances are, you might already know this example. If so, please do not roll your eyes, but feel free