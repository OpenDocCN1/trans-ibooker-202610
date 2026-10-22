![](bbox=[0.13, 0.07, 0.88, 0.21])

Figure 5-2. The LogVisit operation updating the data and notifying the caller of the operation’s success or failure

Although the execute method is of type void, that is, it doesn’t return any data, it still communicates whether the operation has succeeded or failed: if it failed, the caller will get an exception. What if the method succeeds, but the communication of the result to the caller fails? For example:

+   - If LogVisit is part of a REST service and there is a network outage; or
+   - If both LogVisit and the caller are running in the same process, but the process fails before the caller gets to track successful execution of the LogVisit action?

In both cases, the consumer will assume failure and try calling LogVisit again. Executing the LogVisit logic again will result in an incorrect increase of the counter’s value. Overall, it will be increased by 2 instead of 1. As in the previous two examples, the code fails to implement the transaction script pattern correctly, and inadvertently leads to corrupting the system’s state.

As in the previous example, there is no simple fix for this issue. It all depends on the business domain and its needs. In this specific example, one way to ensure transactional behavior is to make the operation idempotent: that is, leading to the same result even if the operation repeated multiple times.

For example, we can ask the consumer to pass the value of the counter. To supply the counter’s value, the caller will have to read the current value first, increase it locally, and then provide the