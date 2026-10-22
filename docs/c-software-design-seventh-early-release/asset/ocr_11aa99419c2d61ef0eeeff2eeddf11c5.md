Creator

virtual factoryMethod() = 0

product = factoryMethod();

Product

High-level
(stable, low
dependencies)

Architectural
Boundary

Low-level
(volatile, malleable,
high dependencies)

ConcreteProduct

ConcreteCreator

virtual factoryMethod()

return new ConcreteProduct();

Figure 3-2. The GoF Factory Method design pattern.