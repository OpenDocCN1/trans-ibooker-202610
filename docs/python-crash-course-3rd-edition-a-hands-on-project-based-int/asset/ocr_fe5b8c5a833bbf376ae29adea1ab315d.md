uniquely named URL pattern in that namespace. The namespace comes from the value we assigned to `app_name` in the *learning_logs/urls.py* file.

In a simple HTML page, a link is surrounded by the *anchor tag* `<a>`:

```
<a href="link_url">link text</a>
```

Having the template tag generate the URL for us makes it much easier to keep our links up to date. We only need to change the URL pattern in *urls.py*, and Django will automatically insert the updated URL the next time the page is requested. Every page in our project will inherit from *base.xhtml*, so from now on, every page will have a link back to the home page.

On the last line, we insert a pair of `block` tags ②. This block, named `content`, is a placeholder; the child template will define the kind of information that goes in the `content` block.

A child template doesn’t have to define every block from its parent, so you can reserve space in parent templates for as many blocks as you like; the child template uses only as many as it needs.

> **NOTE**
> 
> *In Python code, we almost always use four spaces when we indent. Template files tend to have more levels of nesting than Python files, so it’s common to use only two spaces for each indentation level.*

# The Child Template