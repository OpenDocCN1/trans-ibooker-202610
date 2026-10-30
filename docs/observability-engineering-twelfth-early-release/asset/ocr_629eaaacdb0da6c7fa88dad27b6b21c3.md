However, when that same concept is adapted for use with squishier virtual software systems, it opens up a radically different way of interacting with and understanding the code you write.

# # Applying Observability to Software Systems

Kálmán’s definition of observability can be applied to modern software systems. When adapting the concept of observability to software, we must also layer additional considerations that are specific to the software engineering domain.

For a software application to have observability, you must be able to do the following:

+   - Understand the inner workings of your application
+   - Understand any system state your application may have gotten itself into, even new ones you have never seen before and couldn’t have predicted
+   - Understand the inner workings and system state solely by observing and interrogating with external tools
+   - Understand the internal state *without* shipping any new custom code to handle it (because that implies you needed prior knowledge to explain it)

A good litmus test for determining whether those conditions are true is to ask yourself the following questions:

+   - Can you continually answer open-ended questions about the inner workings of your applications to explain any anomalies, without hitting investigative dead ends (i.e., the issue might be in a certain group of things, but you can’t break it down any further to confirm)?
+   - Can you understand what any particular user of your software may be experiencing at any given time?