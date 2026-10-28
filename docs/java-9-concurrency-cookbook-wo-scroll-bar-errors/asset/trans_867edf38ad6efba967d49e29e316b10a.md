

## 准备工作

本食谱的示例使用 Eclipse IDE 实现。如果您使用 Eclipse 或其他 IDE，例如 NetBeans，请打开它并创建一个新的 Java 项目。

## 如何做...

按照以下步骤实现示例：

- 1. 创建一个名为 `Item` 的类，用于表示从发布者发送给订阅者的信息项。该类有两个 `String` 属性，名为 `title` 和 `content`，以及获取和设置其值的 `get()` 和 `set()` 方法。其源代码非常简单，因此此处不予包含。

- 2. 然后，创建一个名为 `Consumer1` 的类，并指定其实现参数化为 `Item` 类的 `Subscriber` 接口。我们需要实现四个方法。首先，实现 `onComplete()` 方法。它只是向控制台写入一条消息：

```
public class Consumer1 implements Flow.Subscriber<Item> {

    @Override
    public void onComplete() {
        System.out.printf("%s: Consumer 1: Completed\n",
            Thread.currentThread().getName());
    }
}
```

- 3. 然后，我们实现 `onError()` 方法。它只是向控制台写入有关错误的<|message|>息：

```
@Override
public void onError(Throwable exception) {
    System.out.printf("%s: Consumer 1: Error\n",
        Thread.currentThread().getName());
    exception.printStackTrace(System.err);
}
```

- 4. 然后，我们实现 onNext() 方法。它只是向控制台写入有关接收到的项的信息：

```
@Override
public void onNext(Item item) {
    System.out.printf("%s: Consumer 1: Item received\n", Thread.currentThread().getName());
    System.out.printf("%s: Consumer 1: %s\n", Thread.currentThread().getName(), item.getTitle());
    System.out.printf("%s: Consumer 1: %s\n", Thread.currentThread().getName(), item.getContent());
}
```

- 5. 最后，我们实现 onSubscribe() 方法。它只是向控制台写入一条消息，并且不使用 Subscription 对象的 request() 方法请求任何项：

```
@Override
public void onSubscribe(Flow.Subscription subscription) {
    System.out.printf("%s: Consumer 1: Subscription received\n", Thread.currentThread().getName());
    System.out.printf("%s: Consumer 1: No Items requested\n", Thread.currentThread().getName());
}
```

- 6. 现在，是 Consumer2 类的时候了。指定其也实现参数化为 Item 类的 Subscriber 接口。在这种情况下，我们有一个私有的 Subscription 属性来存储订阅对象。onComplete() 和 onError() 方法与 Consumer1 类中的方法等效：

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

- 7. onNext() 方法多了一行用于请求另一个元素：

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

- 8. onSubscribe() 方法也多了一行用于请求第一个元素：

```
@Override
public void onSubscribe(Flow.Subscription subscription) {
    System.out.printf("%s: Consumer 2: Subscription received\n",
        Thread.currentThread().getName());
    this.subscription = subscription;
    subscription.request(1);
}
```

- 9. 现在，实现一个名为 Consumer3 的类，并指定其实现参数化为 Item 类的 Subscriber 接口。onComplete() 和 onError() 方法与前几个类中的方法等效：

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

- 10. 在这种情况下，onNext() 方法会向控制台写入有关该项的信息，但不会请求任何元素：

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

- 11. 在 onSubscribe() 方法中我们请求三个项：

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

- 12. 最后，使用 main() 方法实现 Main 类。首先，创建三个消费者，每个类一个：

```
public class Main {
    public static void main(String[] args) {

        Consumer1 consumer1=new Consumer1();
        Consumer2 consumer2=new Consumer2();
        Consumer3 consumer3=new Consumer3();
```

- 13. 现在，创建一个参数化为 Item 类的 SubmissionPublisher 对象，并使用 subscribe() 方法添加三个消费者：

```
SubmissionPublisher<Item> publisher=new SubmissionPublisher<>();

publisher.subscribe(consumer1);
publisher.subscribe(consumer2);
publisher.subscribe(consumer3);
```

- 14. 现在，创建十个 Item 对象并使用 SubmissionPublisher 对象的 submit() 方法发布它们。每个项之间等待一秒：

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

- 15. 最后，使用 close() 方法关闭发布者：

```
publisher.close();
}
```

## 它是如何工作的...

响应式流的主要目标是提供一种处理具有非阻塞背压的异步数据流的机制。我们希望信息接收者优化其资源。由于该机制是异步的，接收者无需使用资源来查找新元素。当新元素到来时，它们将被调用。非阻塞背压允许接收者在接收者准备好时才消费新元素，因此它们可以使用有界队列来存储传入元素，并且不会被新元素的生产者饱和。

Java 中的响应式流基于三个接口：

- `Flow.Publisher`：该接口只有一个方法：
  - `subscribe()`：该方法接收一个 `Subscriber` 对象作为参数。发布者应在发布 Item 时考虑该订阅者。
- `Flow.Subscriber`：该接口有四个方法：
  - `onComplete()`：当 `Publisher` 完成执行时将调用此方法
  - `onError()`：当出现必须通知订阅者的错误时将调用此方法
  - `onNext()`：当 `Publisher` 有新元素时将调用此方法
  - `onSubscribe()`：当发布者使用 `subscribe()` 方法添加订阅者时将调用此方法
- `Flow.Subscription`：该接口有一个方法：
  - `request()`：该方法由 `Subscriber` 用于向发布者请求元素

请注意，这些只是接口，您可以按自己的意愿实现和使用它们。假设的流程如下：

- 1. 有人调用 `Publisher` 的 `subscribe()` 方法，向其发送一个 `Subscriber`。
- 2. `Publisher` 创建一个 `Subscription` 对象并将其发送到 `Subscriber` 的 `onSubscribe()` 方法。
- 3. `Subscriber` 使用 `Subscription` 的 `request()` 方法向 `Publisher` 请求元素。
- 4. 当发布者有元素要发布时，它会将其发送给所有已请求元素的 `Subscribers`，调用它们的 `onNext()` 方法。
- 5. 当发布者结束执行时，它会调用订阅者的 `onComplete()` 方法。

Java API 提供了实现 Publisher 接口并实现此行为的 `SubmissionPublisher` 类。

