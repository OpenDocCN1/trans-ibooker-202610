The pointer could be to a static variable, a global variable, or memory that's been allocated from the heap to ensure that the object being pointed to still exists when the exception is handled. Now, we have solved the problem of keeping the exception object alive. But when the handler has finished with it, what does the catcher do with it?

The catcher of the exception isn't aware about the creation of the exception object (**global**, **static**, or **heap**) and so it doesn't know whether it should delete the received pointer or not. Thus, throwing-by-pointer is not the recommended approach of throwing exceptions.

An object that is thrown will be copied to the created temporary exception object and handed off to the handler. When the exception has been handled, the temporary object will simply be destroyed, and the program will continue. There is no ambiguity as to what to do with it. Therefore, the best practice is to throw the **exception by value**.

# Standard Library Exceptions

The C++ Standard Library defines **std::exception** as the base class for all the Standard Library exceptions. The standard defines the following first-level hierarchy of **exceptions/errors** (the number in brackets indicates how many exceptions derive from that class):

![](bbox=[0.12, 0.64, 0.92, 0.75])

Figure 3.17: Standard Library exception hierarchy (two levels)

These exceptions are used through the C++ Standard Library including the STL. The best practice when creating your own exception class is deriving it from one of the standard exceptions. As we will see next, your special exception can be caught by a handler for one of the standard exceptions.