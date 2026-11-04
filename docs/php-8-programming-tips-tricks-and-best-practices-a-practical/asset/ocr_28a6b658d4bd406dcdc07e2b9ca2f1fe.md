```
```
999 smccormi : Budta:PH:1273715 : 2021-04-20 10:55:55
From User: ajenkins
Elapsed Time: 1.8595671653748
```
```

The most notable feature of the output is the elapsed time. If you look back at the previous section, you'll note that the API running as a traditional PHP application using Apache took about 3.35 seconds to complete 1,000 iterations, whereas the same API, running under Swoole, completes in approximately 1.86 seconds: almost half the time!

Please note that this is without any additional optimization. Swoole has many other features we could use, including the ability to define in-memory tables, spawn tasks off additional worker threads, and use event loops to facilitate caching, among other possibilities. As you can see, Swoole immediately provides a performance boost and is well worth investigating as a possible way to gain even more performance out of your existing applications.

Now that you have an idea of how Swoole might be used to improve application performance, let's have a look at other potential PHP async solutions.

# Using selected PHP frameworks in async mode

There are a number of other PHP frameworks that implement the asynchronous programming model. In this section, we cover ReactPHP, the most popular of the PHP async frameworks, as well as Amp, another popular PHP async framework. In addition, we show you how selected PHP frameworks can be used in async mode.

It's important to note that many of the PHP frameworks able to operate in asynchronous mode have a dependency on the Swoole extension. The one that does not have this dependency is ReactPHP, covered next.

# Working with ReactPHP

ReactPHP (https://reactphp.org/) is an implementation of the Reactor software design pattern and was inspired by the non-blocking asynchronous Node.js framework (https://nodejs.org/en/), among others.

Although ReactPHP does not give you the automatic performance increase seen with the Swoole extension, it has a big advantage in that it does not rely upon features of UNIX or Linux, and can thus run on a Windows server. The other advantage of ReactPHP is that it has no specific dependency on PHP extensions, other than those already included as standard extensions.