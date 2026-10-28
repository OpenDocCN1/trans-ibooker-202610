Then, you executed 100 PollTask tasks to remove elements from the deque. Each one of these tasks removes 10,000 elements from the deque using the pollFirst() and pollLast() methods. The pollFirst() method returns and removes the first element of the deque, and the pollLast() method returns and removes the last element of the deque. If the deque is empty, they return a null value. When all the tasks had finished, you wrote the number of elements of the deque in the console. At that moment, the list had zero elements. Take into account that the ConcurrentLinkedDeque data structure doesn't allow you to add null values.

To write the number of elements in the deque, you used the size() method. You have to take into account that this method can return a value that is not real, especially if you use it when there are threads adding to or deleting data from the list. The method has to traverse the entire deque to count the elements, and the contents of the list can change with this operation. Only if you use them when there aren't any threads modifying the deque, you will have the guarantee that the returned result would be correct.

# There's more...

The ConcurrentLinkedDeque class provides more methods to get elements from the deque:

+   - getFirst() and getLast(): These methods return the first and last element from the deque, respectively. They don't remove the returned element from the deque. If the deque is empty, they throw a NoSuchElementException exception.
+   - peek(), peekFirst(), and peekLast(): These methods return the first and the last element of the deque, respectively. They don't remove the returned element from the deque. If the deque is empty, they return a null value.
+   - remove(), removeFirst(), and removeLast(): These methods return the first and last element of the deque, respectively. They remove the returned element as well. If the deque is empty, they throw a NoSuchElementException exception.