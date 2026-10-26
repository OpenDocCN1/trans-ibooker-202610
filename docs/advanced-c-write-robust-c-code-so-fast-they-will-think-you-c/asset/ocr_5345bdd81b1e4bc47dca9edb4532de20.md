They will continue their job, but nobody knows about that. Then, we used the std::this_thread::sleep_for(2s) function to make the main thread wait for two seconds. We did that because when the main thread finishes its job, the application will stop, and we won't be able to view the output of the detached threads in the terminal. The following screenshot is an example of the output to the terminal:

![](bbox=[0.13, 0.2, 0.93, 0.36])

Figure 5.6: The result of an example execution

In your IDE, the output can change as the order of execution is undefined. A real-world example of asynchronous execution can be an internet browser wherein you can open multiple tabs. When a new tab is opened, the application starts a new thread and detaches them. Although the threads work independently, they can share some resources, such as a file handler, to write logs or do something else.

> Note
> std::thread has a member function called get_id() that returns the unique ID of the std::thread instance. If the std::thread instance wasn't initialized or was joined or detached, get_id() returns a default std::thread::id object. This means that no thread of execution is associated with the current std::thread instance.

Let's use some pseudocode to show an example where computations are done by another computational unit. For example, let's say we develop an application that performs calculations with currency exchange. The user inputs an amount in one currency, chooses another currency to exchange, and the application shows them the amount in that currency. The application, in the background, sends a request to a remote server that holds all the currency exchange rates.