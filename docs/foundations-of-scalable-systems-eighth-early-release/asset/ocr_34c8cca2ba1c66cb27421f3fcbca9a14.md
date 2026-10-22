is on its way - delayed simply due to congestion in the network/server. These faults are collectively known as *crash faults*⁹.

The typical solution that clients adopt to handle crash faults is to resend the request after a configured timeout period. This however is fraught with danger, as Figure 3-6 illustrates. The client sends a request to the server to deposit money in a bank account. When it receives no response after a timeout period, it resends the request. What is the resulting balance? The server may have applied the deposit, or it may not, depending on the partial failure scenario.

![](bbox=[0.16, 0.34, 0.79, 0.7])

Figure 3-6. Client retries a request after timeout

The chance that the deposit may occur twice is a fine outcome for the customer. The bank though is unlikely to be amused by this possibility. Hence, we need a way to ensure in our server operations implementation that retried, duplicate requests from clients only result in the request being applied once. This is necessary to maintain correct application semantics.