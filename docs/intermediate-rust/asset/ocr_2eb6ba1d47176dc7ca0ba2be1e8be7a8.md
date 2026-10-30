# 9

# ASYNCHRONOUS PROGRAMMING

![](bbox=[0.14, 0.48, 0.32, 0.66])

Asynchronous programming is, as the name implies, programming that is not synchronous. At a high level, an asynchronous operation executes in the background—the program won’t wait for the asynchronous operation to complete, but will instead continue on to the next line of code immediately. If you’re not already familiar with asynchronous programming, that definition may feel insufficient as it doesn’t actually explain what asynchronous programming is. To really understand the asynchronous programming model and how it works in Rust, we have to first dig into what the alternative is. That is, we need to understand the synchronous programming model before we can understand the asynchronous one. This is important in both clarifying the concepts and demonstrating the trade-offs of using asynchronous programming: an asynchronous solution is not always the right one! We’ll