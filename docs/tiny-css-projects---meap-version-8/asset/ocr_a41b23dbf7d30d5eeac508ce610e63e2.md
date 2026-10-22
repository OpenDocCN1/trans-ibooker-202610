# 8.1 Getting Started

Regardless of the screen size, we are going to use the same HTML. We will have one stylesheet, and use media queries to adjust how our elements look depending on the screen size. Media queries are used to conditionally apply styles to elements; we will explore these in more detail later in the chapter.

Our HTML consists of two sections, one for the cart and one for the summary, which are wrapped in a container that we will use on wide screens to place the sections side by side. The cart section contains a heading and a table which contains each of the items in the cart. The summary section contains a heading, a definition list, and 2 links. Figure 8.2 shows a diagram of the HTML elements.

![](bbox=[0.18, 0.3, 0.89, 0.58])

Figure 8.2 Diagram of HTML elements

Listing 8.1 is an abridged version of HTML we are starting with. The code mentions a JavaScript file. It exists to calculate and insert total values based on the quantity applied to the input fields but does not affect the styles. Some HTML elements also have ARIA (Accessible Rich Internet Application) added to ensure our code is accessible. ARIA provides a framework to add attributes which help identify features, relationships, and interactions not otherwise conveyed by the code which is used by assistive technologies such as screen readers. It does not have any impact on styles either but we always want to make sure our code is accessible so that we do not prevent disabled users from accessing and using our applications.

# Listing 8.1 Starting HTML

```
<body>
  <main>
    <h1>Checkout</h1>
```