
## We Will Ultimately Lose the War

One aspect of performance that can be counterintuitive (and depressing) is that the performance of every application can be expected to decrease over time—meaning over new release cycles of the application. Often, that performance difference is not noticed, because hardware improvements make it possible to run the new programs at acceptable speeds.

Think what it would be like to run the Windows 10 interface on the same computer that used to run Windows 95. My favorite computer ever was a Mac Quadra 950, but it couldn’t run macOS Sierra (and if it did, it would be so very, very slow compared to Mac OS 7.5). On a smaller level, it may seem that Firefox 69.0 is faster than earlier versions, but those are essentially minor release versions. With its tabbed browsing and synced scrolling and security features, Firefox is far more powerful than Mosaic ever was, but Mosaic can load basic HTML files located on my hard disk about 50% faster than Firefox 69.0.

Of course, Mosaic cannot load actual URLs from almost any popular website; it is no longer possible to use Mosaic as a primary browser. That is also part of the general point here: particularly between minor releases, code may be optimized and run faster. As performance engineers, that’s what we can focus on, and if we are good at our job, we can win the battle.

That is a good and valuable thing; my argument isn’t that we shouldn’t work to improve the performance of existing applications. But the irony remains: as new features are added and new standards adopted—which is a requirement to match competing programs—programs can be expected to get larger and slower.

I’m not advocating that you should never add a new feature or new code to your product; clearly benefits result from enhancing programs. But be aware of the trade-offs you are making, and when you can, streamline.

## Oh, Go Ahead, Prematurely Optimize

Donald Knuth is widely credited with coining the term *premature optimization*, which is often used by developers to claim that the performance of their code doesn’t matter, and if it does matter, we won’t know that until the code is run. The full quote, if you’ve never come across it, is “We should forget about small efficiencies, say about 97% of the time; premature optimization is the root of all evil.”³

3 There is some dispute over who said this originally, Donald Knuth or Tony Hoare, but it appears in an article by Knuth entitled “Structured Programming with goto Statements.” And in context, it is an argument for optimizing code, even if it requires inelegant solutions like a goto statement.

The point of this dictum is that in the end, you should write clean, straightforward code that is simple to read and understand. In this context, optimizing is understood to mean employing algorithmic and design changes that complicate program structure but provide better performance. Those kinds of optimizations indeed are best left undone until such time as the profiling of a program shows that a large benefit is gained from performing them.

What optimization does not mean in this context, however, is avoiding code constructs that are known to be bad for performance. Every line of code involves a choice, and if you have a choice between two simple, straightforward ways of programming, choose the better-performing one.

At one level, this is well understood by experienced Java developers (it is an example of their art, as they have learned it over time). Consider this code:

```
log.log(Level.FINE, "I am here, and the value of X is "
    + calcX() + " and Y is " + calcY());
```

This code does a string concatenation that is likely unnecessary, since the message won’t be logged unless the logging level is set quite high. If the message isn’t printed, unnecessary calls are also made to the calcX() and calcY() methods. Experienced Java developers will reflexively reject that; some IDEs will even flag the code and suggest it be changed. (Tools aren’t perfect, though: the NetBeans IDE will flag the string concatenation, but the suggested improvement retains the unneeded method calls.)

This logging code is better written like this:

```
if (log.isLoggable(Level.FINE)) {
    log.log(Level.FINE,
        "I am here, and the value of X is {} and Y is {}",
        new Object[]{calcX(), calcY()});
}
```

This avoids the string concatenation altogether (the message format isn’t necessarily more efficient, but it is cleaner), and there are no method calls or allocation of the object array unless logging has been enabled.

Writing code in this way is still clean and easy to read; it took no more effort than writing the original code. Well, OK, it required a few more keystrokes and an extra line of logic. But it isn’t the type of premature optimization that should be avoided; it’s the kind of choice that good coders learn to make.

Don’t let out-of-context dogma from pioneering heroes prevent you from in a list, and when a new response came in, it processed the entire list in order to calculate the 90th percentile response time (if that term is unfamiliar, see Chapter 2).

Performance issues can be caused by any part of the entire system where an application is deployed. Common case analysis says to consider the newest part of the system first (which is often the application in the JVM), but be prepared to look at every possible component of the environment.

On the other hand, don’t overlook that initial analysis. If the database is the bottleneck (and here’s a hint: it is), tuning the Java application accessing the database won’t help overall performance at all. In fact, it might be counterproductive. As a general rule, when load is increased into a system that is overburdened, performance of that system gets worse. If something is changed in the Java application that makes it more efficient—which only increases the load on an already overloaded database—overall
