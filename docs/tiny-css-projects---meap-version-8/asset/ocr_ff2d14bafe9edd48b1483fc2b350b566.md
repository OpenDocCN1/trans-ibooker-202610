# 8.2.2 Links and Buttons

We have several links and buttons on this page but stylistically, they all look like buttons. They can be generally categorized by their purpose.

+   - Primary Call To Action: the proceed to checkout link
+   - Secondary Call To Action: Continue Shopping
+   - Destructive: Button with an X to remove items from the cart

We are going to use this categorization to name our classes in order for our rules to be easily reusable. In this chapter, we are only dealing with a single page, but this is an anomaly. In a full application we have multiple pages or components which will reuse the same styles, so instead of naming our class something like "proceed-to-checkout" we are going to use "primary" so that the class can easily be reused in a different context.

Before we address the differences between each of them, let’s first consider their similarities and write a baseline for all of our links that look like buttons (which were given a class of button) and buttons. Once we have written a baseline, we will make rules for each of the button types above.

To create our baseline, the first thing we will do is remove the default gray background set by the browser using background: none. Next we add padding.

We have 2 options on how we set our padding.

+   1. We can set each side individually using padding-top, padding-right, padding-bottom, and padding-right.
+   2. We can use the shorthand padding property which allows us to pass 1 to 4 values.

Figure 8.6 shows the breakdown of how the values are applied when using the shorthand property.

![](bbox=[0.17, 0.6, 0.88, 0.75])

Figure 8.6 Padding property shorthand

We will use the shorthand property. We will pass 2 values (1ex and 1ch) to the padding property, the first will handle the top and bottom padding, while the second will handle left to right.