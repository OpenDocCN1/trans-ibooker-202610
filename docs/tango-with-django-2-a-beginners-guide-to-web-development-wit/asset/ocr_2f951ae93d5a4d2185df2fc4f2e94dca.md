+   - within each view, we could access the `Category` object, get all the categories, and return that in the context dictionary.

However, this is a pretty nasty solution because we will need to include the same code in all views. A [DRY](https://en.wikipedia.org/wiki/Don%E2%80%99t_repeat_yourself) solution would be to create custom template tags that are included in the template, and which can request *their* data.

# Using Template Tags

Create a directory `<workspace>/tango_with_django_project/rango/templatetags`, and create two new modules within it. One must be called `__init__.py`, and this will be kept blank. Name the second module `rango_template_tags.py`. Add the following code to `rango_template_tags.py`.

```
```python
from django import template
from rango.models import Category

register = template.Library()

@register.inclusion_tag('rango/categories.html')
def get_category_list():
    return {'categories': Category.objects.all()}
```
```

There is a new method called `get_category_list()` in this snippet. This method returns a dictionary with one key/value pairing. `categories` represents a list of all the `Category` objects present in the database. From the `register.inclusion_tag()` decorator above, you will see that `rango/categories.html` is referred to – another new template. This new template is used by the Django template engine to render the list of categories you provide in the dictionary that is returned in the function. This rendered list can then be injected into the response of the view that initially called the template tag!

Now would be a good time to create the new template, `categories.html`, and add the following HTML markup. Note the similarity in the name of the template with the existing `category.html` template. This is done by design to make sure you are kept on your toes!

> [^10]: https://en.wikipedia.org/wiki/Don%E2%80%99t_repeat_yourself