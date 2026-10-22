powerful feature and if used properly you can do incredible things with it. However, of course you remember the Peter Parker Principle:

> *With great power comes great responsibility.*
> 
> —Peter Parker, aka Spider-Man

The problem is the “if used properly” part. Inheritance has proven to be hard to use properly (definitely harder than we are lead to believe, see my reasonings above) and thus is misused unintentionally. It is also overused as many developers have the habit to use it for every kind of problem.¹³ This overuse appears to be the source of many problems, as Michael Feathers remarks:¹⁴

> [Programming by difference]¹⁵ fell out of favor in the 1990s when many people in the OO community noticed that inheritance can be rather problematic if it is overused.
> 
> —Michael Feathers

In many situations, inheritance is neither the right approach nor the right tool. Instead, most of the time, it is preferable to use composition instead. You should not be surprised by that revelation, though, because you have already seen it to be true. Composition is the reason the OO-form of the *Strategy* design pattern works so well, not inheritance. It is the introduction of an abstraction and the aggregation of according data members that make the *Strategy* design pattern so powerful, not the inheritance-based implementation of different strategies. In fact, you will find that many design patterns are firmly based on composition, not on inheritance (see for instance the *Strategy* design pattern in “Guideline 19: Use Strategy to Isolate How Things are Done”, the *Observer* design pattern in “Guideline 25: Apply Observers as an Abstract Notification Mechanism”, the *Adapter* design pattern in “Guideline 24: Use Adapters to Standardize Interfaces”, the *Decorator* design pattern in “Guideline 35: Use Decorators to Add Customization Hierarchically”, or the *Bridge* design pattern in “Guideline 28: Build Bridges to Remove Physical Dependencies”). All of these enable extension by means of inheritance, but are themselves enabled by means of composition.