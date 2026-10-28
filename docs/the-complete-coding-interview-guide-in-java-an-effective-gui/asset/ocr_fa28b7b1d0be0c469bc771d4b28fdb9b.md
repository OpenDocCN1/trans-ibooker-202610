Let's see this in terms of code:

```
```java
public class MyQueueViaStack<E> {
	private final Stack<E> stackEnqueue;
	private final Stack<E> stackDeque;
	public MyQueueViaStack() {
		stackEnqueue = new Stack<>();
		stackDeque = new Stack<>();
	}
	public void enqueue(E e) {
		stackEnqueue.push(e);
	}
	public E dequeue() {
		reverseStackEnqueue();
		return stackDeque.pop();
	}
	public E peek() {
		reverseStackEnqueue();
		return stackDeque.peek();
	}
	public int size() {
		return stackEnqueue.size() + stackDeque.size();
	}
	private void reverseStackEnqueue() {
		if (stackDeque.isEmpty()) {
			while (!stackEnqueue.isEmpty()) {
				stackDeque.push(stackEnqueue.pop());
			}
		}
	}
}
```
```