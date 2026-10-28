# Hints

+   - Start refactoring the `about.html` template first.
+   - Update the `title_block` then the `body_block` in each template.
+   - To check if a category exists in your templates, remember you can simply use the conditional statement `{% if category %}` – so long as the `category` object is passed by that name in your template context!
+   - Have the development server running and check the page as you work on it. Don’t change the whole page to find it doesn’t work. Changing things incrementally and testing those changes as you go is a much safer solution.
+   - To reference the links to category pages, you can use the following template code, paying particular attention to the Django template `{% url %}` command.

```
<a href="{% url 'rango:show_category' category.slug %}">
    {{ category.name }}
</a>
```

# 8.4 The `render()` Method and the `request` Context

When writing views we have used some different methods, the preferred way being the Django shortcut method `render()`. The `render()` method requires that you pass through the `request` as the first argument. The `request` context houses a lot of information regarding the session (including information about the user, for example). Refer to the official Django Documentation on Request objects⁹.

By passing the `request` through to the template, this means that you will also have access to such information when creating templates. In the next chapter, we will access information about the `user` – but for now, check through all of your views and make sure that they have been implemented using the `render()` method. Otherwise, your templates won’t have the information we need later on.

⁹https://docs.djangoproject.com/en/2.1/ref/request-response/#httprequest-objects