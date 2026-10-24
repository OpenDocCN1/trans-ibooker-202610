
As you can see from Figure 5.24, no time elapsed between the start of our Stopwatch object and the creation of our lazy object. The lazy behavior is ensured by the Lazy keyword, which is a built-in C# instruction. However, when calling explicitly the property of our DemoClass object through _lazy.Value.DemoProperty, we can see that our second printout of the elapsed time shows 5060 milliseconds. This proves that the actual initialization of the DemoClass object was delayed until we called one of its properties (it is the same with methods, of course). The same program without the lazy pattern looks like this:

```
static void Main(string[] args){
    Stopwatch sw = new Stopwatch();
    sw.Start();
    DemoClass _notLazy = new DemoClass();
    Console.WriteLine(sw.ElapsedMilliseconds);
    Console.Read();
}
```

Figure 5.25 – Non-lazy initialization

Now, we instantiate the same DemoClass object the normal way. The following screenshot shows the outcome:

![](img/78092c233296cbd7723546ae74eec12d_98_0.png)

Figure 5.26 – Non-lazy DemoClass object

## Lazy loading wrap-up

The direct instantiation causes the program to take about 5 seconds before it prompts the user for a key entry. Because the lazy pattern is very simple, there is no real need to add a wrap-up section, but let’s still see some of its benefits and drawbacks, as follows:

- The major benefit of the lazy pattern is to prevent a waste of resources when the concrete instances of a given object are not needed.
- This major benefit might also be its main drawback. If you were to use lazy loading for everything, you would not have a clear picture of the actual resources your application needs to fully function. Make sure to evaluate this when testing the application.

Let’s now go through some of the behavioral patterns.

## Strategy pattern

The strategy pattern, part of the behavioral category, is used to let consumer classes decide on which algorithm to choose from a family of related algorithms. The purpose of this pattern is to avoid if-else or switch constructs directly in the client, to handle different implementations, and let it pick the right strategy instead. Here is a class diagram of the sample that comes next:

![](img/78092c233296cbd7723546ae74eec12d_99_0.png)

Figure 5.27 – Strategy pattern

The Program class is our consumer. FormatMessageStrategy is our context class, which lets the consumer specify at runtime which strategy to use. IMessageFormatter is our strategy contract, while EvGridMessageFormatter and BrokerMessageFormatter are different implementations of that strategy. This is comparable to what we have seen before, but I added some validation bits to the picture. Here, we have two different concrete strategies to format messages according to their target recipient. Let’s see the strategy pattern in code.

### Strategy pattern in action

The strategy pattern helps prevent the following code constructs in the consumer class:

```
if (msg.MessageType == Message.MessageTypes.Bus){
    //write code to format bus messages
}
else if (msg.MessageType == Message.MessageTypes.Grid){
    //write code to format grid messages
}
```

Figure 5.28 – Strategy pattern prevents inline decisions

Here, you would add some formatting logic directly into the client. The goal of the strategy pattern is instead to delegate this logic to dedicated strategies and choose the appropriate one at runtime. Here is the code of the strategy contract:

```
public interface IMessageFormatter{
    string FormatMessage(string message);
}
```

Figure 5.29 – Strategy contract

This is a very simple contract that specifies that every strategy should implement the FormatMessage method. Here, we can see two strategies that implement our contract:

```
public class EvGridMessageFormatter : IMessageFormatter{
    public string FormatMessage(string message){
        Console.WriteLine("in EvGridMessageFormatter");
        //let's pretend we formatted the message for evengrid
        return message;
    }
}
```

```
public class BrokerMessageFormatter : IMessageFormatter{
    public string FormatMessage(string message){
        Console.WriteLine("in BrokerMessageFormatter");
        //let's pretend we formatted the message for a bus
        return message;
    }
}
```

Figure 5.30 – Concrete strategies

We pretend to format the message differently according to the strategy. Now comes our context class, the one that is used by our consumer class, as follows:

```
public class FormatMessageStrategy{
    private IMessageFormatter _strategy;
    public FormatMessageStrategy(IMessageFormatter strategy){
        _strategy = strategy;
    }
    public string Format(string message){
        return _strategy.FormatMessage(message);
    }
}
```

Figure 5.31 – Context class

The context class has a private member of the IMessageFormatter type. Its constructor takes a strategy as input and assigns it to the private member. This lets the consumer class specify which strategy to use at runtime. The Format method can be called by the consumer class and, in turn, calls the FormatMessage method of the strategy that was passed in. Now, the next code block shows how that context class can be used from within the consumer class:

```
static async Task Main(string[] args){
    IEventPublisher brokerPublisher = new BrokerPublisher();
    await brokerPublisher.PublishMessage(
        new FormatMessageStrategy(new BrokerMessageFormatter()).Format("a message"));
    IEventPublisher gridPublisher = new EvGridPublisher();
    await gridPublisher.PublishMessage(
        new FormatMessageStrategy(new EvGridMessageFormatter()).Format("a message"));
    Console.Read();
}
```

Figure 5.32 – Strategy consumer code

We subsequently call both strategies. The following screenshot shows the output of the console program when executed:

![](img/78092c233296cbd7723546ae74eec12d_101_0.png)

Figure 5.33 – Strategy pattern console output

### Strategy pattern wrap-up

You might be wondering how the DI method differs from the strategy pattern, and you would be right to do so. Both patterns enable us to pass concrete objects to client classes through method injection. A big difference between the strategy pattern and DI is the category to which they belong. Remember that DI is a part of the creational patterns and, as such, they enter into play at the time objects get created.

The strategy pattern is a part of the behavioral category, and as such, it is intended to implement multiple behaviors to react to application events, such as user inputs or button clicks. When you use the strategy pattern, it is systematically with the intention of implementing multiple concrete classes of a given contract, while this is not especially the case (other than in unit tests) with DI only.

Classes that participate in the communication all inherit from Person, which in turn holds a reference to the concrete mediator. Most examples that illustrate this design pattern look alike. I tried to innovate a little bit, but we will see another example in Chapter 7, Architectural Trends and Global Summary, about using the mediator pattern together with Command Query Request Segregation (CQRS).

### Mediator in action

As always, we start by defining our contract, as follows:

```
interface IMediator{
    public abstract void Register(Person p);
    public abstract void Send(string from, string message, audience to);
}
```

Figure 5.35 – Mediator contract

The contract is an interface or an abstract class. We then implement the concrete mediator, as illustrated here:

```
class ConcreteMediator : IMediator{
    private Dictionary<string, Person> _persons =
        new Dictionary<string, Person>();
    public void Register(Person p){
        _persons.TryAdd(p.Name, p);
        p.concreteMediator = this;
    }
    public void Send(
        string from, string message, audience to){
        var persons = (audience.adult == to) ? _persons.Values.Where(
            p=>p.Name != from && p.GetType().Equals(typeof(Adult))) :
            _persons.Values.Where(p=>p.Name != from);
        if(persons.Count()>0)
            foreach (var person in persons)
                person.Receive(from, message);
    }
}
```

Figure 5.36 – Concrete mediator
