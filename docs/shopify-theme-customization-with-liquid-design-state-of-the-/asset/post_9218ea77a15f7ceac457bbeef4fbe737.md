
# The reversed parameter

The final parameter on our list is `reversed`, which, as its name suggests, allows us to reverse the order of iteration. The reversed parameter does not have any value representation, and we can include it at the end of the opening `for` loop tag:

```
```jinja
{% for item in (3..5) reversed %}
    {{ item }}
{% endfor %}
```
```

In the previous example, we have added the reversed parameter as a secondary parameter next to our `range` parameter, resulting in our tag performing iteration in reverse order:

```
```text
5 4 3
```
```

As we have had the chance to see for ourselves, parameters are a powerful addition to the `for` tag, which we can use to achieve the necessary type of iteration and get only the results we require.

## The cycle tag

`cycle` is another powerful tag that we can only use in combination with the `for` tag to loop over a group of strings and output them to each iteration in the specific order in which they were initially defined. We can define the `cycle` tag by opening curly brace delimiters with a percentage symbol, followed by the word `cycle`, followed by any number of strings separated by commas. Finally, we close the `cycle` tag with closing brace delimiters and a percentage symbol:

```
```jinja
{% for product in collection.products limit: 4 %}
    <div class="product-item {% cycle "first", "", "", "last" %}"></div>
{% endfor %}
```
```

In the previous example, we have included a `cycle` tag with four different strings inside a `for` loop, which we have limited to four iterations. By implementing the `cycle` tag, we have ensured that our first `div` element will receive a class `first`, the fourth element will receive a `last` class, while the two elements in between will receive no classes as the two strings for those positions are empty:

```
```html
<div class="product-item first"></div>
<div class="product-item"></div>
<div class="product-item"></div>
```

```
<div class="product-item last"></div>
```

From our result, we can see that the cycle tag is quite a valuable tool that we can use to pass data to a specific iteration within a loop in an order that we have defined. However, what would happen if we removed the limit from our loop or increased it to have nine iterations instead?

```
{% for product in collection.products limit: 9 %}
    <div class="product-item {% cycle "first", "", "", "last" %}"></div>
{% endfor %}
```

In our previous example, we had the exact same number of iterations as the number of strings inside the cycle tag. In our newest example, the number of iterations is higher than the number of strings, which means that the cycle will reset and start applying strings again, in order, for as many iterations as needed:

```
<div class="product-item first"></div>
<div class="product-item"></div>
<div class="product-item"></div>
<div class="product-item last"></div>
<div class="product-item first"></div>
<div class="product-item"></div>
<div class="product-item"></div>
<div class="product-item last"></div>
<div class="product-item first"></div>
```

So far, we have learned the basic use of the cycle tag and that the tag will continue to output strings for as many iterations as possible, which is quite helpful if we have a single cycle tag, but what if we have two or more?

```
{% for product in collection.products limit: 6 %}
    <div class="product-item {% cycle "first", "", "", "last" %}"></div>
{% endfor %}
```

```
{% for product in collection.products limit: 4 %}
    <div class="product-item {% cycle "first", "", "", "last" %}"></div>
{% endfor %}
```

In the previous example, we have created two separate for loops, the first one limited to six iterations and the second limited to four. As we have just learned, no matter how many strings we have defined inside the cycle tag, the tag will continue to output strings for as many iterations as needed:

```
<div class="product-item first"></div>
```

```
<div class="product-item"></div>
```

```
<div class="product-item"></div>
```

```
<div class="product-item last"></div>
```

```
<div class="product-item first"></div>
```

```
<div class="product-item"></div>
```

```
<div class="product-item"></div>
```

```
<div class="product-item last"></div>
```

```
<div class="product-item first"></div>
```

```
<div class="product-item"></div>
```

As we can see from the results, the first for loop has created six iterations in the exact order that we intended. However, the second for loop did not produce the same results. Instead, it continued to output the strings starting from the exact position where the previous cycle tag stopped.

This type of behavior is logical considering that Liquid currently cannot differentiate between different types of cycle tags. However, we can easily resolve this by introducing a parameter called the cycle group.

The cycle group parameter allows us to separate cycle tags by ensuring that each cycle will output strings starting with position one, regardless of whether or not we have already used a cycle tag on the same page. After the cycle keyword, we can include the cycle group parameter by adding the string name encapsulated by parentheses, followed by a colon:

```
{% for product in collection.products limit: 6 %}
```

```
  <div class="product-item {% cycle "group1": "first", "", "", "last" %}"></div>
```

```
{% endfor %}
```

```
{% for product in collection.products limit: 4 %}
```

```
  <div class="product-item {% cycle "group2": "first", "", "", "last" %}"></div>
```

```
{% endfor %}
```

- `render`
- `raw`
- `comment`

## The layout tag

As we recall from the first chapter, when discussing the `Layout` directory, we mentioned the importance of the `theme.liquid` file as it is in this file that we will render all of our files and templates. It is in this file that we arrange the general layout of our pages.

Our pages currently consist of three key elements: **header**, **main content**, and **footer**. Let’s say we wanted to remove the header and footer sections from our product pages, or at the very least to replace them with a different set of headers and footers. *How would we do this?*

1. The first step to achieve this would be to create an alternate layout file that our theme product pages will use instead. We can do this by expanding the **Layout** directory in our code editor and clicking on the **Add a new layout** button, which will trigger a popup to select the type of layout page we are looking to create and select the name for our new page.
2. Within the drop-down menu, we will select the **theme** option as the layout type, and within the following field, we will enter the word `alternate` as our filename. Once we have finished selecting our layout file type and name, click on the **Create layout** button to finalize the process:

![](img/4e10d2f6d65d6d1c551f760d91c2c116_98_0.png)

Figure 3.10 – Example of creating a new layout file

With this, we have successfully created a new layout file named `theme.alternate.liquid`. Suppose we tried to compare this file with our original `theme.liquid`; we would see that they are exactly the same. The reason for this is that when we create a new page, layout, or template, Shopify will not create a new page entirely, but it will copy a default page that our theme is already using. For example, if we were to expand the **Templates** directory and create a new page template, the new page template would contain all of the changes we had previously made to the `page.liquid` file.

Now that we have created a new layout file, we will assign it to our product pages. We can do this by expanding the **Templates** directory and clicking on the `product.liquid` file where we can define the layout for this particular template, which we should include on the very first line of the file. We can define the layout tag with a set of curly brace delimiters with percentage symbols around the word `layout`, followed by the name of the layout file, which we should encapsulate within parentheses:

```
```liquid
{% layout "theme.alternate" %}
```
```

If we were to navigate to the **theme.alternate** template now and delete `{% section 'header' %}` and `{% section 'footer' %}`, the header and footer sections would no longer be visible on our product page. However, they would still be visible on pages that use the original `theme.liquid` layout.

> **Important note:**
> 
> Besides the name of the layout file, the layout tag also accepts the `none` keyword, without parentheses, as its value, in which case the page will use no layout file. In this case, the particular page using this particular template will have no access to any code or file, including `.css`, `.js`, or others that we initially loaded through the `theme.liquid` file.

Any template file that does not contain the layout tag will, by default, use the original `theme.liquid` layout as a failsafe.

## The liquid and echo tags

The `liquid` tag is one of the newest additions to Liquid, and it is quite a powerful one as it allows us to write multiple tags within one set of delimiters, making our code a lot easier to read and maintain. We can define the liquid tag with a curly brace delimiter and a percentage symbol on the left side of the word liquid followed by as many lines of Liquid code as we need. Note that we only need to define an opening `liquid` tag, where the `liquid` closing tag is automatically closed.
