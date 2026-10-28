
If further details are required, then you can write an example. Consider the `Vehicle` superclass having the `move()` static method:

```
```java
public class Vehicle {

    public static void move() {
        System.out.println("Moving a vehicle");
    }
}
```
```

Now, consider the `Car` subclass having the same static method:

```
```java
public class Car extends Vehicle {

    // this method hides Vehicle#move()
    public static void move() {
        System.out.println("Moving a car");
    }
}
```
```

Now, let's call these two static methods from the `main()` method:

```
```java
public static void main(String[] args) {

    Vehicle.move(); // call Vehicle#move()
    Car.move();     // call Car#move()
}
```
```

The output reveals that these two static methods are hiding one another:

```
```text
Moving a vehicle
Moving a car
```
```

Notice that we call static methods via the class name. Calling static methods on instances is a very bad practice, so avoid doing this during an interview!

## Can we write virtual methods in Java?

Yes, we can! Actually, in Java, all non-static methods are, by default, virtual methods. We can write a non-virtual method by marking it with the `private` and/or `final` keyword. In other words, the methods that can be inherited for polymorphic behavior are virtual methods. Or, if we turn the logic of this statement on its head, the methods that cannot be inherited (marked as `private`) and the methods that cannot be overridden (marked as `final`) are non-virtual.

## What is the difference between polymorphism and abstraction?

Abstraction and polymorphism represent two fundamental OOP concepts that are interdependent. Abstraction allows the developer to design general solutions that are reusable and customizable, while polymorphism allows the developer to defer choosing the code that should be executed at runtime. While abstraction is implemented via interfaces and abstract classes, polymorphism relies on overriding and overloading techniques.

## Do you consider overloading an approach for implementing polymorphism?

This is a controversial topic. Some people do not regard overloading as polymorphism; therefore, they do not accept the idea of compile-time polymorphism. Such voices maintain that the only overriding method is genuine polymorphism. The argument behind this statement says that only overriding allows code to behave differently depending on the runtime conditions. In other words, exhibiting polymorphic behavior is the privilege of method overriding. I consider that as long as we understand the premises of overloading and overriding, we also understand how both variants sustain polymorphic behavior.

**Important note**
Questions that tackle controversial topics are delicate and hard to approach correctly. Therefore, it is advisable to jump into the answer directly with this statement *This is a controversial topic.* Of course, the interviewer is interested to hear your opinion as well, but he will be pleased to see that you know both sides of the coin. As a rule of thumb, try to answer in an objective manner and don't approach one side of a coin with radicalism or with a poor arsenal of arguments. Controversial things remain controversial after all, and this is not the proper time and place to demystify them.

OK, now let's continue with some questions based on the SOLID principles and the famous and indispensable **Gang Of Four (GOF)** design patterns. Note that this book doesn't cover GOF design patterns, but there are great books and videos out there that are dedicated to this topic. I recommend that you try *Learn Design Patterns with Java*, by Aseem Jain (`https://www.packtpub.com/application-development/learn-design-patterns-java-video`).

## Which OOP concept serves the Decorator design pattern?

The OOP concept that serves the Decorator design pattern is **Composition**. Via this OOP concept, the Decorator design pattern provides new functionalities without modifying the original class.

## When should the Singleton design pattern be used?

The Singleton design pattern seems to be the proper choice when we need just one application-level (global) instance of a class. Nevertheless, a Singleton should be used with precaution because it increases the coupling between classes and can become a bottleneck during development, testing, and debugging. As the famous *Effective Java* points out, using Java enums is the best way of implementing this pattern. It is a common scenario to rely on a Singleton pattern for global configurations (for example, loggers, `java.lang.Runtime`), hardware access, database connections, and so on.

> **Important note**
> 
> Whenever you can cite or mention famous references, do so.

## What is the difference between the Strategy and State design patterns?

The State design pattern is meant to do a certain thing depending upon the *state* (it exhibits certain behaviors in different *states* without changing the class). The Strategy design pattern, on the other hand, is meant to be used for switching between a range of algorithms without modifying the code that uses it (the client uses algorithms interchangeably via composition and runtime delegation). Moreover, in State, we have a clear order of *state* transition (the flow is created by linking each *state* to another state), while in Strategy, the client can choose the algorithm that it wants in any order. For example, the State pattern can define the *states* of sending a package to a client.

The package starts from the *ordered state*, and continues with the *delivered state* and so on until it passes through each state and reaches the final *state* when the client has *received* the package. On the other hand, the Strategy pattern defines different strategies for accomplishing each state (for example, we may have different strategies for delivering the package).

## What is the difference between the Proxy and Decorator patterns?

The Proxy design pattern is useful for providing an access control gateway to something. Commonly, this pattern creates proxy objects that stand in place of the real object. Each request for the real object must pass through the proxy objects, which decides how and when to forward it to the real object. The Decorator design pattern never creates an object, it just decorates an existing object at runtime with new functionality. While chaining proxies is not an advisable practice, chaining decorators in a certain order exploits this pattern in the right way. For example, while the Proxy pattern can represent a proxy server for the internet, the Decorator pattern can be used to decorate the proxy server with different custom settings.

## What is the difference between the Facade and Decorator patterns?

While the Decorator design pattern is meant to add new functionalities to an object (in other words, to decorate the object), the Facade design pattern doesn't add new functionalities at all. It just facades the existing functionalities (hides the complexities of a system) and calls them behind the scenes via a *friendly face* exposed to the client. The Facade pattern can expose a simple interface that calls individual components to accomplish complex tasks. For example, the Decorator pattern can be used to build a car by decorating a chassis with an engine, a gearbox, and so on, while the Facade pattern can hide the complexity of building the car by exposing a simple interface for command industrial robots that know the building process details.

## What is the key difference between the Adapter and Bridge patterns?

The Adapter pattern strives to provide compatibility between an existing code that we cannot modify (for example, third-party code) and a new system or interface. On the other hand, the Bridge pattern is implemented upfront and is meant to decouple an abstraction from implementation in order to avoid an insane number of classes. So, Adapter strives to provide compatibility between things after they were designed (think along the lines of A comes from After), while Bridge is built upfront to let the abstraction and the implementation vary independently (think along the lines of B comes from Before). While Adapter acts as the middle man between two systems that work fine independently but cannot communicate with one another (they don’t have compatible input/output), the Bridge pattern enters the scene when our problem can be solved via orthogonal class hierarchy, but we get stuck with scalability issues and limited extension. For example, consider two classes, ReadJsonRequest and ReadXmlRequest, which are capable of reading from several devices, such as D1, D2, and D3. D1 and D2 produce only JSON requests, while D3 produces XML requests only. Via Adapter, we can convert between JSON and XML, meaning that these two classes can communicate with all three devices. On the other hand, via the Bridge pattern, we can avoid ending with many classes such as ReadXMLRequestD1, ReadXMLRequestD2, ReadXMLRequestD3, ReadJsonRequestD1, ReadJsonRequestD2, and ReadJsonRequestD3.

We can continue to compare design patterns until we finish all the possible combinations. The final few of these questions have covered the most popular questions of the type Design Pattern 1 versus Design Pattern 2. It is strongly advisable to challenge yourself with these types of questions and try to identify similarities and differences between two or more given design patterns. Most of the time, these questions use two design patterns from the same category (for example, two structural or two creational patterns), but they can be from different categories as well. In such a case, this is the first statement that the interviewer expects to hear. So, in such cases, start by saying to which category each of the design patterns involved belongs.

Notice that we skipped all simple questions of the type, What is an interface?, What is an abstract class?, and so on. Typically, such questions are avoided since they don’t say much about your understanding level, being more about reciting some definitions. The interviewer can ask What is the main difference between abstract classes and interfaces?, and he can deduce from your answer whether you know what an interface and an abstract class is. Always be prepared to give examples. The inability to shape an example reveals a serious lack of understanding of the essence of things.
