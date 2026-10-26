# Exception Usage Guidelines

When using exceptions in your C++ code, remember the following points:

+   - Chant: **Throw by value and catch by reference**
+   - **Do not use exceptions for normal program flow.** If a function meets an abnormal condition and is not able meet its (functional) obligations, then and only then do you throw an exception. If the function can resolve the abnormal condition and fulfil its obligations, then it is not an exception. They are named exceptions for a reason and you will not incur any processing overhead if you do not use them.
+   - **Do not throw an exception from within a destructor.** Remember that because of stack unwinding, local variable destructors will be executed. If a destructor is called during the stack unwinding process and throws an exception, then the program will terminate.
+   - **Do not swallow exceptions.** Do not use the default catch handler and do nothing about the exception. The exception has been thrown to indicate that there is a problem and you should do something about it. Ignoring an exception can lead to a failure later that will be hard to troubleshoot. This is because any useful information is truly lost with the swallowed exception.
+   - **The exception object is copied from the throw.**

# Resource Management (in an Exceptional World)

So far, we have looked at local variable scope, and how `automatic` and `dynamic lifetime variables` are dealt with when the variable goes out of scope – automatic lifetime variables (those placed on the stack) are fully destructed while `dynamic lifetime variables` (those allocated to the heap by the programmer) are not destructed: we just lose any access to them. We have also seen that, when an exception is thrown, the nearest matching handler is found and all the local variables between the throw point and the handler will be destructed through the stack unwinding process.

We can use this knowledge to write robust resource management classes that will relieve us from the need to keep track of resources (dynamic lifetime variables, file handles, system handles, and so on) to ensure that they are released (back into the wild) when we are done with them. The technique that's utilized to manage resources, both under normal operating and under exceptions, is known as **Resource Acquisition is Initialization (RAII)**.