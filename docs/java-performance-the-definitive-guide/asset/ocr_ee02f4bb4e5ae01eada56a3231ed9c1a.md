Figure 7-5 shows the classic case of a Java memory leak caused by a collection class—in this case, HashMap. (Collection classes are the most frequent cause of a memory leak: the application inserts items into the collection and never frees them.) This is a comparison histogram view: it displays the difference in the number of objects in two heap dumps. For example, 19,744 more Integer objects occur in the target heap dump compared to its baseline.

The best way to overcome this situation is to change the application logic such that items are proactively discarded from the collection when they are no longer needed. Alternatively, a collection that uses weak or soft references can automatically discard the items when nothing else in the application is referencing them, but those collections come with a cost (as is discussed later in this chapter).

![](bbox=[0.14, 0.31, 0.84, 0.76])

Figure 7-5. Histogram comparison

Often when this kind of exception is thrown, the JVM does not exit, because the exception affects only a single thread in the JVM. Let’s look at a JVM with two threads performing a calculation. One of them may get the OutOfMemoryError. By