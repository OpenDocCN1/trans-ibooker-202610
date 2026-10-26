21. Build and run the application. In the Terminal, you will see that log messages appear from the different threads with different logging levels and with useful information. After some time has passed, you will get some output similar to the following:

![](bbox=[0.12, 0.26, 0.92, 0.58])

Figure 6.26: The execution result of the activity project

As you can see, it's really easy to read and understand logs. You can easily change the StreamLogger class to write logs to the file on the filesystem if your needs differ. You can add any other information that you may need to debug your application using logs, such as output function parameters. You can also override the left shift operator for your custom types to output debug information easily.

In this project, we employed many things that we have learned about during this chapter. We created an additional stream for thread-safe output, we formatted the output to the desired representation, we employed std::stringstream to perform formatting data, and we used macro definitions for convenient logger usage. Thus, this project demonstrates our skills in working with concurrent I/O.