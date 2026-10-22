```
int AddTwoNumbers(int a, int b)
{
    return a + b;
}
```

This is the extreme case of a shallow module: the public interface (the method’s signature) and its logic (the methods) are exactly the same. Having such a module introduces extraneous “moving parts,” and thus, instead of encapsulating complexity, it adds accidental complexity to the overarching system.

# Microservices as Deep Modules

Apart from different terminology, the notion of deep modules differs from the microservices pattern in that the modules can denote both logical and physical boundaries, while microservices are strictly physical. Otherwise, both concepts and their underlying design principles are the same.

The services implementing a single business method, shown in Figure 14-3, are shallow modules. Because we had to introduce integration-related public methods, the resultant interfaces are “wider” than they should have been.

From a system complexity standpoint, a deep module reduces the system’s global complexity, while a shallow module increases it by introducing a component that doesn’t encapsulate its local complexity.

Shallow services are also the reason why so many microservices-oriented projects fail. The mistaken definitions of a microservice as a service having no more than X lines of code, or as a service that should be easier to rewrite than to modify, concentrate on the individual service while missing the most important aspect of the architecture: the system.

The threshold upon which a system can be decomposed into microservices is defined by the use cases of the system that the