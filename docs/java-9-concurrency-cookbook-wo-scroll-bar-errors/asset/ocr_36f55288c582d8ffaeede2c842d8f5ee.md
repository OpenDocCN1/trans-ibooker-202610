In the example, first, you have launched the consumers and then the producers, so, as the buffer is empty, all the consumers will be blocked until the producers begin their execution and stores strings in the list.

The following screenshot shows part of the output of an execution of this example:

```
<terminated> Main (71) [Java Application] C:\Program Files\Java\jdk-9\bin\javaw.exe (20 nov. 2016 1:22:08)
Producer: Producer: 62: Producer done
Producer: Producer: 60: Producer done
Producer: Producer: 57: Producer done
Main: Size of the buffer: 0
Main: End of the example
```

To write the number of elements of the buffer, you have used the size() method. You have to take into account that this method can return a value that is not real, if you use them when there are threads adding or deleting data in the list. The method has to traverse the entire list to count the elements and the contents of the list can change for this operation. Only if you use them when there aren't any threads modifying the list, you will have the guarantee that the returned result is correct.

# There's more...

The LinkedTransferQueue class provides other useful methods. The following are some of them:

+   - getWaitingConsumerCount(): This method returns the number of consumers that are blocked in the take() method or poll (long timeout, TimeUnit unit) because the LinkedTransferQueue object is empty.
+   - hasWaitingConsumer(): This method returns true if the LinkedTransferQueue object has consumers waiting, or false otherwise.
+   - offer(E e): This method adds the element passed as a parameter at the end of the LinkedTransferQueue object and returns the true value. E represents the class used to parameterize the declaration of the LinkedTransferQueue class or a subclass of it.