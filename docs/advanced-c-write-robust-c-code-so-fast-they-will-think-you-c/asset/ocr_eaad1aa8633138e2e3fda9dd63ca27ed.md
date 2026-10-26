This technique uses the lifetime of the File objects to acquire and release the resource, ensuring that the resources do not leak. The resource is acquired during the construction (initialization) of the management class and released during the destruction of the management class. It is this behavior of scope-bound resources that gives rise to the name Resource Acquisition Is Initialization.

The preceding example deals with managing file handles that are system resources. It applies to any resources that are required to be acquired before use, and then relinquished when finished. The RAII technique can be applied to a wide range of resources – open files, open pipes, allocated heap memory, open sockets, threads of execution, database connections, locking of mutexes/critical sections – basically any resource that is in short supply in the host system and needs to be managed.

# Exercise 3: Implementing RAII for Memory and File Handles

In this exercise, we will implement two different classes that will manage memory or files using the RAII technique. Follow these steps to implement this exercise:

+   1. Open the Lesson3 project in Eclipse. Then in the Project Explorer, expand Lesson3, then Exercise03, and double click on Exercise3.cpp to open the file for this exercise into the editor.
+   2. Click on the Launch Configuration drop down menu and select New Launch Configuration.... Configure the L3Exercise3 application from the Search Project menu to run it with the name L3Exercise3.
+   3. Click on the Run button to run Exercise 3. This will produce the following output:

```
------ Exercise 3 ------
allocated 20 bytes @ 0x56364f043280
allocated 20 bytes @ 0x56364f0432a0
allocated 20 bytes @ 0x56364f0432c0
allocated 20 bytes @ 0x56364f0432e0
allocated 20 bytes @ 0x56364f043300
------ Complete. ------
Leaked Memory Pointers...
  0: 0x56364f043280
  1: 0x56364f0432a0
  2: 0x56364f0432c0
  3: 0x56364f0432e0
  4: 0x56364f043300
Leaked Files...
  0: HelloB1.txt
  1: HelloB2.txt
```

Figure 3.20: Leaky memory and files from Exercise3.cpp