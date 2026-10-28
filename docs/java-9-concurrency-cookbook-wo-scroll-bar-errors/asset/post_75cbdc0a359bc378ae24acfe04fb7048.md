
## Getting ready

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or other IDE such as NetBeans, open it and create a new Java project.

## How to do it...

Follow these steps to implement the example:

- 1. Create a class named `Item` that will represent the items of information sent from the publisher to the subscribers. This class has two `String` attributes, named `title` and `content`, and the methods to `get()` and `set()` their values. Its source code is very simple, so it won't be included here.

- 2. Then, create a class named `Consumer1` and specify that it implements the `Subscriber` interface parameterized with the `Item` class. We have to implement four methods. First, we implement the `onComplete()` method. It simply writes a message to the console:

```
public class Consumer1 implements Flow.Subscriber<Item> {

    @Override
    public void onComplete() {
        System.out.printf("%s: Consumer 1: Completed\n",
            Thread.currentThread().getName());
    }
}
```

- 3. Then, we implement the `onError()` method. It simply writes information about the error to the console:

```
@Override
public void onError(Throwable exception) {
    System.out.printf("%s: Consumer 1: Error\n",
        Thread.currentThread().getName());
    exception.printStackTrace(System.err);
}
```

- 4. Then, we implement the onNext() method. It simply writes information about the received item to the console:

```
@Override
public void onNext(Item item) {
    System.out.printf("%s: Consumer 1: Item received\n", Thread.currentThread().getName());
    System.out.printf("%s: Consumer 1: %s\n", Thread.currentThread().getName(), item.getTitle());
    System.out.printf("%s: Consumer 1: %s\n", Thread.currentThread().getName(), item.getContent());
}
```

- 5. And finally, we implement the onSubscribe() method. It simply writes a message to the console and doesn't request any item using the request() method of the Subscription object:

```
@Override
public void onSubscribe(Flow.Subscription subscription) {
    System.out.printf("%s: Consumer 1: Subscription received\n", Thread.currentThread().getName());
    System.out.printf("%s: Consumer 1: No Items requested\n", Thread.currentThread().getName());
}
```

- 6. Now, it's time for the Consumer2 class. Specify that it also implements the Subscriber interface parameterized with the Item class. In this case, we have a private Subscription attribute to store the subscription object. The onComplete() and onError() methods are equivalent to the ones of the Consumer1 class:

```
public class Consumer2 implements Flow.Subscriber<Item> {

    private Subscription subscription;

    @Override
    public void onComplete() {
        System.out.printf("%s: Consumer 2: Completed\n", Thread.currentThread().getName());
    }
```

```
@Override
public void onError(Throwable exception) {
    System.out.printf("%s: Consumer 2: Error\n",
        Thread.currentThread().getName());
    exception.printStackTrace(System.err);
}
```

- 7. The onNext() method has an additional line to request another element:

```
@Override
public void onNext(Item item) {
    System.out.printf("%s: Consumer 2: Item received\n",
        Thread.currentThread().getName());
    System.out.printf("%s: Consumer 2: %s\n",
        Thread.currentThread().getName(),
        item.getTitle());
    System.out.printf("%s: Consumer 2: %s\n",
        Thread.currentThread().getName(),
        item.getContent());
    subscription.request(1);
}
```

- 8. The onSubscribe() method also has an additional line to request the first element:

```
@Override
public void onSubscribe(Flow.Subscription subscription) {
    System.out.printf("%s: Consumer 2: Subscription received\n",
        Thread.currentThread().getName());
    this.subscription = subscription;
    subscription.request(1);
}
```

- 9. Now, implement a class called Consumer3 and specify that it implements the Subscriber interface parameterized with the Item class. The onComplete() and onError() methods are equivalent to those of the previous classes:

```
public class Consumer3 implements Flow.Subscriber<Item> {

    @Override
    public void onComplete() {
        System.out.printf("%s: Consumer 3: Completed\n",
            Thread.currentThread().getName());
    }
```

```
@Override
public void onError(Throwable exception) {
    System.out.printf("%s: Consumer 3: Error\n",
        Thread.currentThread().getName());
    exception.printStackTrace(System.err);
}
```

- 10. The onNext() method, in this case, writes information about the item to the console but doesn't request any element:

```
@Override
public void onNext(Item item) {
    System.out.printf("%s: Consumer 3: Item received\n",
        Thread.currentThread().getName());
    System.out.printf("%s: Consumer 3: %s\n",
        Thread.currentThread().getName(),
        item.getTitle());
    System.out.printf("%s: Consumer 3: %s\n",
        Thread.currentThread().getName(),
        item.getContent());
}
```

- 11. In the onSubscribe(), method we request three items:

```
@Override
public void onSubscribe(Flow.Subscription subscription) {
    System.out.printf("%s: Consumer 3: Subscription received\n",
        Thread.currentThread().getName());
    System.out.printf("%s: Consumer 3: Requested three items\n",
        Thread.currentThread().getName());
    subscription.request(3);
}
```

- 12. Finally, implement the Main class with the main() method. First, create three consumers, one of each class:

```
public class Main {
    public static void main(String[] args) {

        Consumer1 consumer1=new Consumer1();
        Consumer2 consumer2=new Consumer2();
        Consumer3 consumer3=new Consumer3();
```

- 13. Now, create a SubmissionPublisher object parameterized with the Item class and add the three consumers using the subscribe() method:

```
SubmissionPublisher<Item> publisher=new SubmissionPublisher<>();

publisher.subscribe(consumer1);
publisher.subscribe(consumer2);
publisher.subscribe(consumer3);
```

- 14. Now, create ten Item objects and publish them using the submit() method of the SubmissionPublisher object. Wait a second between each item:

```
for (int i=0; i<10; i++) {
    Item item =new Item();
    item.setTitle("Item "+i);
    item.setContent("This is the item "+i);
    publisher.submit(item);
    try {
        TimeUnit.SECONDS.sleep(1);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```

- 15. Finally, close the publisher with the close() method:

```
publisher.close();
}
```

## How it works...

The main goal of reactive streams is provide a mechanism to process asynchronous stream of data with non-blocking back pressure. We want that the receivers of information optimize their resources. As the mechanism is asynchronous, receivers don’t need to use their resources to look for new elements. They will be called when a new element comes in. The non-blocking back pressure allows receivers to consume new elements only when the receivers are ready, so they can use a bounded queue to store the incoming elements and they won’t be saturated by producers of new elements.

The reactive streams in Java are based on three interfaces:

- `Flow.Publisher`: This interface has only one method:
  - `subscribe()`: This method receives a `Subscriber` object as parameter. The publisher should take this subscriber into account when it publishes an Item.
- `Flow.Subscriber`: This interface has four methods:
  - `onComplete()`: This method will be called when the `Publisher` has finished its execution
  - `onError()`: This method will be called when there is an error that must be notified to the subscribers
  - `onNext()`: This method will be called when the `Publisher` has a new element
  - `onSubscribe()`: This method will be called when the publisher has added the subscriber with the `subscribe()` method
- `Flow.Subscription`: This interface has one methods:
  - `request()`: This method is used by the `Subscriber` to request an element from the publisher

Take into account that these are only interfaces and you can implement them and use them as you want. The supposed flow is as follows:

- 1. Someone calls the `subscribe()` method of a `Publisher`, sending it a `Subscriber`.
- 2. The `Publisher` creates a `Subscription` object and sends it to the `onSubscribe()` method of the `Subscriber`.
- 3. The `Subscriber` uses the `request()` method of the `Subscription` to request elements to the `Publisher`.
- 4. When the publisher has an element to publish, it sends them to all `Subscribers` that have requested elements, calling their `onNext()` method.
- 5. When the publisher ends its execution, it calls the `onComplete()` method of the subscribers.

Java API provides the `SubmissionPublisher` class that implements the Publisher interface and implements this behavior.
