Oh my, I just realized that this may be a particularly bad example for explaining the *Prototype* design pattern. Slicing animals. This sounds bad. So let us swiftly move on. Where were we: ah yes, we want a copy of Dolly, but we only have an `Animal*`. This is where the *Prototype* design pattern comes into play.

# The Prototype Design Pattern Explained

The *Prototype* design pattern is one of the five creational design patterns collected by the Gang of Four. It is focused on providing an abstract way of creating copies of some abstract entity:

> **THE *PROTOTYPE* DESIGN PATTERN**
> 
> Intent: “Specify the kind of objects to create using a prototypical instance, and create new objects by copying this prototype.” (GoF)

Figure 7-5 shows the original UML formulation, taken from the GoF book: