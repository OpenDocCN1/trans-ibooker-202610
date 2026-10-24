The constructor gets an instance of IEventPublisher as an argument, which in this case is replaced by an instance of BrokerPublisher, as defined in the startup class. It is possible to have multiple concrete classes implementing the same contract at the same time and add them to collections. What is key to remember in this example is that the controller is unaware of the implementation details. It will receive a concrete instance of an object that implements a certain contract. This makes it easier to write unit tests, as we will see in our next section.

# DI code – direct mode

One of the biggest advantages of DI is its positive impact on the testability quality attribute. Let’s see two different ways of testing the controller shown in the previous section. Still in ASP.NET Core, the following method makes use of a mocking framework:

```
[Fact]
0 references
public async Task PublishMessageTest()
{
    var _mockEventPub = new Mock<IEventPublisher>();
    var _demoController = new DemoController(_mockEventPub.Object);
    var actionResult = await _demoController.PublishMessage("test");
    Assert.IsType<AcceptedResult>(actionResult);
}
```

Figure 5.8 – Mocking framework

As you can see, we directly instantiate DemoController with a mocked representation of our IEventPublisher contract. This allows us to inject a fake object instead of the actual BrokerPublisher class we used previously. The reason you want to do this is that you do not want the actual actions to be taken during a unit test. Note that the usage of a mocking framework is not required, but it just makes your life easier. Here is an alternative implementation:

```
[Fact]
0 references
public async Task PublishMessageTestWithBasicDirectInjection(){
    var _demoController = new DemoController(new TestPublisher());
    var actionResult = await _demoController.PublishMessage("test");
    Assert.IsType<AcceptedResult>(actionResult);
}
```

Figure 5.9 – Alternative mocking framework