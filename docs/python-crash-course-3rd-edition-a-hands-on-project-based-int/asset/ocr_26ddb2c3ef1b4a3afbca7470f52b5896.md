pushes this group of links to the right side of the browser window.

The if block is the same conditional block we used earlier to display appropriate messages to users, depending on whether they’re logged in ❷. The block is a little longer now because there are some styling rules inside the conditional tags. The greeting for authenticated users is wrapped in a `<span>` element ❸. A *span element* styles pieces of text or elements of a page that are part of a longer line. While div elements create their own divisions in a page, span elements are continuous within a larger section. This can be confusing at first, because many pages have deeply nested div elements. Here, we’re using the span element to style informational text on the navigation bar: in this case, the logged-in user’s name.

In the else block, which runs for unauthenticated users, we include the links for registering a new account and logging in ❹. These should look just like the link to the topics page.

If you wanted to add more links to the navigation bar, you’d add another `<li>` item to one of the `<ul>` groups that we’ve defined, using styling directives like the ones you’ve seen here.

Now let’s add the logout form to the navigation bar.

# Adding the Logout Form to the Navigation Bar

When we first wrote the logout form, we added it to the bottom of *base.xhtml*. Now let’s put it in a better place, in the navigation bar:

*base.xhtml*

```
--snip--
</ul> <!-- End of account-related links -->

{% if user.is_authenticated %}
```