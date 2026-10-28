
![](img/6709a71136a6b327c831c6c15aff2f6a_621_0.png)

Furthermore, we apply step 1 again since 9 (the front of the given queue) is bigger than the last element that’s added to the extra queue (8). So, 9 goes in the extra queue, as shown in the following diagram:

![](img/6709a71136a6b327c831c6c15aff2f6a_621_1.png)

Next, 3 is smaller than 9, so we must dequeue and enqueue it back in the given queue, as shown in the following diagram:

![](img/6709a71136a6b327c831c6c15aff2f6a_621_2.png)

At this point, we’ve processed (visited) all the elements from the given queue, so we apply step 3 of our algorithm. We dequeue all the elements from the extra queue and enqueue them in the given queue, as shown in the following diagram:

![](img/6709a71136a6b327c831c6c15aff2f6a_621_3.png)

Now, we repeat the whole process until the given queue is sorted in ascending order. Let's see the code:

```
public static void sort(Queue<Integer> queue) {
if (queue == null || queue.size() < 2) {
return;
}

// this is the extra queue
Queue<Integer> extraQueue = new ArrayDeque<>();

int count = 0; // count the processed elements
boolean sorted = false; // flag when sorting is done

int queueSize = queue.size(); // size of the given queue
int lastElement = queue.peek(); // we start from the front
// of the given queue

while (!sorted) {
// Step 1
if (lastElement <= queue.peek()) {
lastElement = queue.poll();
extraQueue.add(lastElement);
} else { // Step 2
queue.add(queue.poll());
}
}

// still have elements to process
count++;
if (count != queueSize) {
continue;
}

// Step 4
if (extraQueue.size() == queueSize) {
sorted = true;
}
}
```

```
}

// Step 3
while (extraQueue.size() > 0) {
    queue.add(extraQueue.poll());
    lastElement = queue.peek();
}

count = 0;
}
}
```

This code's runtime is O(n²). The complete application is called *SortQueueViaTempQueue*.

## Coding challenge 7 – Sorting a queue without extra space

**Amazon, Google, Adobe, Microsoft, Flipkart**

**Problem**: Consider you’ve been given a queue of integers. Write a snippet of code that sorts this queue without using extra space.

**Solution**: In the preceding problem, we had to solve the same problem but using an extra queue. This time, we cannot use an extra queue, so we must sort the queue in place.

We can think of sorting as a continuous process of finding the minimum element from the given queue, extracting it from its current position, and adding it to the rear of this queue. Expanding this idea may result in the following algorithm:

- 1. Consider the current minimum as `Integer.MAX_VALUE`.
- 2. Dequeue an element from the unsorted part of the queue (initially, the unsorted part is the entire queue).
- 3. Compare this element with the current minimum.
- 4. If this element is smaller than the current minimum, then do the following:
  - a. If the current minimum is Integer.MAX_VALUE, then this element becomes the current minimum and we do not enqueue it back in the queue.
  - b. If the current minimum is not Integer.MAX_VALUE, then we enqueue the current minimum back in the queue and this element becomes the current minimum.
- 5. If this element is bigger than the current minimum value, then we enqueue it back in the queue.
- 6. Repeat from step 2 until the whole unsorted part is traversed.
- 7. At this step, the current minimum is the minimum of the entire unsorted part, so we enqueue it back in the queue.
- 8. Set the new boundary of the unsorted part and repeat from step 1 until the unsorted part size is 0 (every time we execute this step, the unsorted part’s size is decreased by 1).

The following diagram is a snapshot of this algorithm for the queue; that is, rear → 3 → 9 → 1 → 8 → 5 → 2 → front:

![](img/6709a71136a6b327c831c6c15aff2f6a_624_0.png)

![](img/6709a71136a6b327c831c6c15aff2f6a_624_1.png)

![](img/6709a71136a6b327c831c6c15aff2f6a_624_2.png)

![](img/6709a71136a6b327c831c6c15aff2f6a_624_3.png)

![](img/6709a71136a6b327c831c6c15aff2f6a_624_4.png)

Figure 14.19 – Sorting a queue without extra space

Notice how each minimum of the unsorted part (initially, the whole queue) is added back into the queue and becomes a member of the sorted part of the queue. Let's see the code:

```
public static void sort(Queue<Integer> queue) {

    // traverse the unsorted part of the queue
    for (int i = 1; i <= queue.size(); i++) {

        moveMinToRear(queue, queue.size() - i);
    }
}

// find (in the unsorted part) the minimum
// element and move this element to the rear of the queue
private static void moveMinToRear(Queue<Integer> queue,
        int sortIndex) {

    int minElement = Integer.MAX_VALUE;
    boolean flag = false;

    int queueSize = queue.size();
    for (int i = 0; i < queueSize; i++) {

        int currentElement = queue.peek();

        // dequeue
        queue.poll();

        // avoid traversing the sorted part of the queue
        if (currentElement <= minElement && i <= sortIndex) {

            // if we found earlier a minimum then
            // we put it back into the queue since
            // we just found a new minimum
            if (flag) {
                queue.add(minElement);
            }
```

```
flag = true;
minElement = currentElement;
} else {
// enqueue the current element which is not the minimum
queue.add(currentElement);
}
}

// enqueue the minimum element
queue.add(minElement);
}
```

This code's runtime is O(n²). The complete application is called *SortQueueWithoutExtraSpace.*

## Coding challenge 8 – Sorting a stack with the help of another stack

**Amazon, Google, Adobe, Microsoft, Flipkart**

**Problem:** Consider you’ve been given an unsorted stack. Write a snippet of code that sorts the stack in descending or ascending order. You can only use an additional temporary stack.

**Solution:** If we could use two additional stacks, then we could implement an algorithm that repeatedly searches the minimum value in the given stack and pushes it into the final or resulting stack. A second additional stack will be used as a buffer while searching the given stack. However, the problem requires us to use only one additional temporary stack.

Due to this constraint, we are forced to pop from the given stack (let’s denote it as s1) and push in order into the additional stack (let’s denote it as s2). To accomplish this, we use a temporary or auxiliary variable (let’s denote it as t), as shown in the following diagram (the given stack is top → 1 → 4 → 5 → 3 → 1 → 2):

# Figure 14.20 – Sorting a stack

![](img/6709a71136a6b327c831c6c15aff2f6a_627_0.png)

The solution consists of two main steps:

- 1. While s1 is not empty, do the following:
  - a. Pop a value from s1 and store it in t (action 1 in the previous diagram shows this for value 3).
  - b. Pop from s2 and push it into s1 as long as what we pop from s2 is bigger than t or s2 is not empty (action 2 in the previous diagram).
  - c. Push t into s2 (action 3 in the previous diagram).
- 2. Once step 1 is complete, s1 is empty and s2 is sorted. The biggest value is at the bottom, so the resulting stack is top → 5 → 4 → 3 → 2 → 1 → 1. The second step consists of copying s2 into s1. This way, s1 is sorted in the reverse order of s2, so the smallest value is at the top of s1 (top → 1 → 1 → 2 → 3 → 4 → 5).

Let's see the code:

```
public static void sort(Stack<Integer> stack) {
    Stack<Integer> auxStack = new Stack<>();
    // Step 1 (a, b and c)
    while (!stack.isEmpty()) {
        int t = stack.pop();
        while (!auxStack.isEmpty() && auxStack.peek() > t) {
            stack.push(auxStack.pop());
        }
    }
```

```
}

auxStack.push(t);
}

// Step 2
while (!auxStack.isEmpty()) {
    stack.push(auxStack.pop());
}
}
```

The complete code is called *SortStack*.

## Coding challenge 9 – Sorting a stack in place

**Amazon, Google, Adobe, Microsoft, Flipkart**

**Problem:** Consider you’ve been given an unsorted stack. Write a snippet of code that sorts the stack in place. Note that a variation of this problem will explicitly mention that you cannot use any repetitive statements, such as `for`, `while`, and so on.

**Solution:** In the preceding problem, we had to solve the same problem but using an explicit extra stack. This time, we cannot use an explicit extra stack, so we must sort the stack in place.

Let’s consider that the given stack is top → 4 → 5 → 3 → 8 → 2 →1. The solution starts by popping the values from the stack until the stack is empty. Afterward, we insert the values from the recursion call stack back into the given stack in sorted position.

Let’s try to apply this approach to our stack. The following diagram reveals the process of popping the values from the stack until the stack is empty. On the left-hand side, we have the initial state. On the right-hand side, we have the result:
