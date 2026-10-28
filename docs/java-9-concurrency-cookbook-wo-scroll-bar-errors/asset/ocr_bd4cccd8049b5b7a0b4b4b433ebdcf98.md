The reactive streams in Java are based on three interfaces:

+   - `Flow.Publisher`: This interface has only one method:
+     - `subscribe()`: This method receives a `Subscriber` object as parameter. The publisher should take this subscriber into account when it publishes an Item.
+   - `Flow.Subscriber`: This interface has four methods:
+     - `onComplete()`: This method will be called when the `Publisher` has finished its execution
+     - `onError()`: This method will be called when there is an error that must be notified to the subscribers
+     - `onNext()`: This method will be called when the `Publisher` has a new element
+     - `onSubscribe()`: This method will be called when the publisher has added the subscriber with the `subscribe()` method
+   - `Flow.Subscription`: This interface has one methods:
+     - `request()`: This method is used by the `Subscriber` to request an element from the publisher

Take into account that these are only interfaces and you can implement them and use them as you want. The supposed flow is as follows:

+   1. Someone calls the `subscribe()` method of a `Publisher`, sending it a `Subscriber`.
+   2. The `Publisher` creates a `Subscription` object and sends it to the `onSubscribe()` method of the `Subscriber`.
+   3. The `Subscriber` uses the `request()` method of the `Subscription` to request elements to the `Publisher`.
+   4. When the publisher has an element to publish, it sends them to all `Subscribers` that have requested elements, calling their `onNext()` method.
+   5. When the publisher ends its execution, it calls the `onComplete()` method of the subscribers.

Java API provides the `SubmissionPublisher` class that implements the Publisher interface and implements this behavior.