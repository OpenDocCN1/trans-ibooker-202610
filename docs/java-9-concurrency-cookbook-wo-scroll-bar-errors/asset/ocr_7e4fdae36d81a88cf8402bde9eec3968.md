```
items[i][j].setName("Item "+i+" "+j);
}
```

9. Then, create a MySpliterator object to process all the elements of the matrix:

```
MySpliterator mySpliterator=new MySpliterator(items, 0,
    items.length);
```

10. Finally, use the stream() method of the StreamSupport class to create a stream from Spliterator. Pass the true value as the second parameter to indicate that our stream will be in parallel. Then, use the forEach() method of the Stream class to write information about each element:

```
StreamSupport.stream(mySpliterator, true).forEach( item -> {
    System.out.printf("%s: %s\n",Thread.currentThread()
        .getName(),item.getName());
});
}
```

# How it works...

The main element of this example is Spliterator. This interface defines methods that can be used to process and partition a source of elements to be used, for example, the source of a Stream object. You will rarely need to use a Spliterator object directly. Only if you want a different behavior--that is, if you want to implement your own data structure and create Stream from it--use a Spliterator object.

Spliterator has a set of characteristics that defines its behavior. They are as follows:

+   - CONCURRENT: The data source can be safely modified concurrently
+   - DISTINCT: All the elements of the data source are distinct
+   - IMMUTABLE: Elements can be added, deleted, or replaced in the data source
+   - NONNULL: There's no null element in the data source
+   - ORDERED: There's an encounter ordered in the elements of the data source
+   - SIZED: The value returned by the estimateSize() method is the exact size of the Spliterator
+   - SORTED: The elements of Spliterator are sorted
+   - SUBSIZED: After you call the trySplit() method, you can obtain the exact size of both the parts of Spliterator