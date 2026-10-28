The following screenshot shows the output of the example and you can see how the behavior of the reactive streams is as expected:

![](bbox=[0.23, 0.17, 0.77, 0.41])

The three Subscriber objects receive their Subscription. As Consumer1 doesn't request any Item, it won't receive them. Consumer3 has requested three, so you will see in the output of the example that it will receive those three Item objects. Finally, the Consumer2 object will receive the ten Item objects and the notification about the end of execution of the Publisher.

# There's more...

There is an additional interface that should be used with reactive streams. It's the Flow.Processor interface and groups the Flow.Publisher and the Flow.Subscriber interfaces. Its main purpose is to be an element between a publisher and a subscriber to transform the elements produced by the first one into a format that can be processed by the second one. You can have more than one processors in a chain so the output of one of them could be processed by the next one.

Java also defines the Flow class that includes the four interfaces explained before.