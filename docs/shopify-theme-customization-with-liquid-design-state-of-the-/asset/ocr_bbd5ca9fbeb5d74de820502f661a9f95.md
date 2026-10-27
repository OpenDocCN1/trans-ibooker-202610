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

# The liquid and echo tags

The `liquid` tag is one of the newest additions to Liquid, and it is quite a powerful one as it allows us to write multiple tags within one set of delimiters, making our code a lot easier to read and maintain. We can define the liquid tag with a curly brace delimiter and a percentage symbol on the left side of the word liquid followed by as many lines of Liquid code as we need. Note that we only need to define an opening `liquid` tag, where the `liquid` closing tag is automatically closed.