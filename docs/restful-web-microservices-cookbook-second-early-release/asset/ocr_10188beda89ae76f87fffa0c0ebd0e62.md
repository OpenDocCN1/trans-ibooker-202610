# 3.16 Having A Goal In Mind

There are times when the client application needs to continue running (making requests, performing work, sending updates, etc.) until a certain goal is reached (a queue is empty, the total has reached the proper level, etc.). This is especially true for client apps that have some level of autonomy or automated processing. There are some challenges to this including cases where the *client* knows the goal but the *service(s)* being used by that client do not.

# Problem

How do you program a client application to continue processing until some “goal” is reached? Also, what does it take to create client applications that have a goal that is “private” and not shared or understood by any services?

# Solution

In order to build client applications that can continue operations until a “goal” is reached, you need to program-in some level of autonomy for that client. The application needs to know what property (or properties) to monitor, needs to know the target value(s) for the selected properties, need to be able to locate and monitor the values over time, and know when an “end” has been reached and how to effect a “stop” in processing.

Programming this solution is essentially the work of most gaming engines or autonomous robot systems. Client applications can follow the PAGE model³⁷: Percepts, Actions, Goals, Environment:

+   - **Percepts** are the properties that need to be monitored
+   - **Actions** are the tasks clients can commit in order to affect the values of the properties (read, write, compute)
+   - **Goals** are the desired end values
+   - **Environment** is the “playing field” or “game space” in which the client operates. In our case, this would be the services with which the client