In summary, the *Adapter* design pattern can be considered one of the most valuable design patterns to combine different pieces of functionality and make them work together. I promise that it will prove to be a valuable tool in your daily work. Still, do not abuse the power of *Adapter* in some heroic effort to combine apples and oranges (or even oranges and grapefruits: they are similar, but not the same). Always be aware of *LSP* expectations.

# GUIDELINE 24: USE *ADAPTERS* TO STANDARDIZE INTERFACES

+   - Apply the *Adapter* design pattern with the intent to adapt interfaces, so that otherwise incompatible pieces can work together;
+   - Be aware that *Adapter* is useful for both dynamic and static polymorphism;
+   - Distinguish between *object adapters*, *class adapters*, and *function adapters*;
+   - Understand the differences between the *Adapter* and *Strategy* design pattern;
+   - Pay attention to *LSP* violations when using the *Adapter* design pattern.

# Guideline 25: Apply Observers as an Abstract Notification Mechanism

Chances are good that you have heard about observers before. “Oh, yes of course I have, isn’t this what the so-called social media platforms are doing with us?”, you are asking. Well, not exactly what I was going for, but yes, I believe we could call these platforms observers. And yes, there is also a pattern to what they do, even though it is not a design pattern. But I am