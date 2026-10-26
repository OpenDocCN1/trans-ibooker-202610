> Note
> The solution for this activity can be found on page 696.

# Summary

In this chapter, we learned about I/O operations in C++. We considered the I/O Standard Library, which provides an interface for synchronous I/O operations. Also, we considered platform-dependent native tools for asynchronous I/O, and the Boost.Asio library for cross-platform asynchronous I/O operations. We also learned how to use I/O streams in multithreaded applications.

We started by looking at the basic features that the Standard Library provides for I/O operations. We learned about predefined stream objects such as std::cin and std::cout. In practice, we learned how to work with standard streams and override shift operators for easy read and write custom data types.

Next, we practiced how to create additional streams. We inherited from the basic stream class, implemented our own stream buffer class, and practiced their usage in the exercises. We learned about the most appropriate stream classes for inheritance and considered their pros and cons.

Then, we considered the approaches of asynchronous I/O operations on different operating systems. We briefly considered using the cross-platform I/O library known as Boost.Asio, which provides an interface for both synchronous and asynchronous operations.

Finally, we learned about how to perform I/O operations in multithreaded applications. We put all of these new skills into practice by building a multithreaded logger. We created a logger abstraction and employed it in the Art Gallery simulator. As a result, we created an easy, clear, and robust logging system that allows us to easily debug the application using logs. In all, we employed everything we learned about in this chapter.

In the next chapter, we're going to be taking a closer look at testing and debugging applications. We will start by learning about assertions and safety netting. Then, we will practice writing unit tests and mocks for interfaces. After, we will practice debugging applications in the IDE: we will use breakpoints, watchpoints, and data visualization. Finally, we will write an activity that will master our skills of testing code.