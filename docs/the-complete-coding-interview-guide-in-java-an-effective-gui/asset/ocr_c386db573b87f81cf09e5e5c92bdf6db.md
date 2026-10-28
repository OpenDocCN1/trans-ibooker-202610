We will quickly go over Big O notation and highlight the things that matter the most. Next, we'll jump into examples that have been carefully crafted to cover a wide range of problems, and so by the end of this chapter, you'll be able to determine and express Big O for almost any given snippet of code. Our agenda includes the following:

+   - Analogy
+   - Big O complexity time
+   - The best case, worst case, and expected case
+   - Big O examples

So, let's start our Big O journey!

# Analogy

Imagine a scenario where you've found one of your favorite movies on the internet. You can order it or download it. Since you want to see it as soon as possible, which is the best way to proceed? If you order it, then it will take a day to arrive. If you download it, then it will take half a day to download. So, it is faster to download it. That's the way to go!

But wait! Just when you get ready to download it, you spot the *Lord of the Rings Master Collection* at a great price, and so you think about downloading it as well. Only this time, the download will take 2 days. However, if you place an order, then it will still only take a single day. So, placing an order is faster!

Now, we can conclude that no matters how many items we order, the shipping time remains constant. We call this O(1). This is a constant runtime.

Moreover, we conclude that the download time is directly proportional to the file sizes. We call this O(n). This is an asymptotic runtime.

From day-to-day observations, we can also conclude that online ordering scales better than online downloading.

**This is exactly what Big O time means: an asymptotic runtime measurement or an asymptotic function.**

As an asymptotic measurement, we are talking about Big O complexity time (this can be complexity space as well).