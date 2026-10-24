The command-line flags to tune the generation sizes all adjust the size of the young generation; the old generation gets everything that is left over. A variety of flags can be used to size the young generation:

+   - `-XX:NewRatio=N`  
+     Set the ratio of the young generation to the old generation.  
+   
+   - `-XX:NewSize=N`  
+     Set the initial size of the young generation.  
+   
+   - `-XX:MaxNewSize=N`  
+     Set the maximum size of the young generation.  
+   
+   - `-XmnN`  
+     Shorthand for setting both `NewSize` and `MaxNewSize` to the same value.

The young generation is first sized by the `NewRatio`, which has a default value of 2. Parameters that affect the sizing of heap spaces are generally specified as ratios; the value is used in an equation to determine the percentage of space affected. The `NewRatio` value is used in this formula:

```
Initial Young Gen Size = Initial Heap Size / (1 + NewRatio)
```

Plugging in the initial size of the heap and the `NewRatio` yields the value that becomes the setting for the young generation. By default, then, the young generation starts out at 33% of the initial heap size.

Alternately, the size of the young generation can be set explicitly by specifying the `NewSize` flag. If that option is set, it will take precedence over the value calculated from the `NewRatio`. There is no default for this flag since the default is to calculate it from `NewRatio`.

As the heap expands, the young generation size will expand as well, up to the maximum size specified by the `MaxNewSize` flag. By default, that maximum is also set using the `NewRatio` value, though it is based on the maximum (rather than initial) heap size.

Tuning the young generation by specifying a range for its minimum and maximum sizes ends up being fairly difficult. When a heap size is fixed (by setting `-Xms` equal to `-Xmx`), it is usually preferable to use `-Xmn` to specify a fixed size for the young generation as well. If an application needs a dynamically sized heap and requires a larger (or smaller) young generation, then focus on setting the `NewRatio` value.