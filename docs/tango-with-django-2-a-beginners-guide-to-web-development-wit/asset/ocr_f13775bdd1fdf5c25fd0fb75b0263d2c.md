Django’s form handling machinery processes the data returned from a user’s browser via an HTTP POST request. It not only handles the saving of form data into the chosen model but will also automatically generate any error messages for each form field (if any are required). This means that Django will not store any submitted forms with missing information that could potentially cause problems for your database’s [referential integrity](https://en.wikipedia.org/wiki/Referential_integrity). For example, supplying no value in the category name field will return an error, as the field cannot be blank.

From the `render()` call, you’ll see that we refer to `add_category.html` – a new template (we define this below!). This will contain the relevant Django template code and HTML for the form and page.

# ## Creating the *Add Category* Template

Create the file `templates/rango/add_category.html`. Within the file, add the following HTML markup and Django template code.

```
```html
<!DOCTYPE html>
<html>
    <head>
        <title>Rango</title>
    </head>

    <body>
        <h1>Add a Category</h1>
        <div>
            <form id="category_form" method="post" action="/rango/add_category/">
                {% csrf_token %}
                {% for hidden in form.hidden_fields %}
                    {{ hidden }}
                {% endfor %}
                {% for field in form.visible_fields %}
                    {{ field.errors }}
                    {{ field.help_text }}
                    {{ field }}
                {% endfor %}
                <input type="submit" name="submit" value="Create Category" />
            </form>
        </div>
    </body>
</html>
```
```