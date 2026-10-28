```
if (stackOfMin.isEmpty()) {
		return Integer.MAX_VALUE;
	} else {
		return stackOfMin.peek();
	}
}
```

Done! Our solution performed in O(1) complexity time. The complete application is called MinStackConstantTime. A problem related to this one requires you to implement the same functionality in constant time and space. The solution to this problem imposes several restrictions, as follows:

+   - The pop() method returns void to avoid returning incorrect values.
+   - The given value multiplied by 2 should not exceed the int data type domain.

In short, these restrictions are caused by the solution itself. We cannot use extra space; therefore, we will use the initial stack of values to store the minimum as well. Moreover, we need to multiply the given value by 2, so we should ensure that the int domain is not exceeded. Why do we need to multiply the given value by 2?

Let's bring some light to this subject! Let's assume that we need to push a value into a stack that has a certain minimum value. If this value is bigger than or equal to the current minimum value, then we can simply push it into the stack. But if it is smaller than the minimum, then we push 2*value - minimum, which should be smaller than the value itself. Then, we update the current minimum as value.

When we pop a value, we have to consider two aspects. If the popped value is bigger or equal to the minimum, then this is the real value that was pushed earlier. Otherwise, the popped value is not the pushed value. The real pushed value is stored in the minimum. After we pop the top of the stack (the minimum value), we have to restore the previous minimum. The previous minimum is obtained as 2*minimum - top. In other words, since the current top is 2*value - previous_minimum and the value is the current minimum, the previous minimum is 2*current_minimum - top. The following code illustrates this algorithm:

```
public class MyStack {
	private int min;
	private final Stack<Integer> stack = new Stack<>();
}
```