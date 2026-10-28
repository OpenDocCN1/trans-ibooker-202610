# 5.7 Creating a Population Script

Entering test data into your database tends to be a hassle. Many developers will add in some bogus test data by randomly hitting keys, like wTFzmN00bz7. Rather than do this, it is better to write a script to automatically populate the database with **realistic and credible data**. This is because when you go to demo or test your app, you’ll need to be able to see some credible examples in the database. If you’re working in a team, an automated script will mean each collaborator can simply run that script to initialise the database on their computer with the same sample data as you. It’s therefore good practice to create what we call a *population script*.

To create a population script, create a new file called `populate_rango.py`. Create this file in `<workspace>/tango_with_django_project/`, or in other words, your Django project’s root directory. When the file has been created, add the following code.

```
import os
os.environ.setdefault('DJANGO_SETTINGS_MODULE',
    'tango_with_django_project.settings')

import django
django.setup()
from rango.models import Category, Page

def populate():
    # First, we will create lists of dictionaries containing the pages
    # we want to add into each category.
    # Then we will create a dictionary of dictionaries for our categories.
    # This might seem a little bit confusing, but it allows us to iterate
    # through each data structure, and add the data to our models.

    python_pages = [
        {'title': 'Official Python Tutorial',
        'url':'http://docs.python.org/3/tutorial/'},
        {'title':'How to Think like a Computer Scientist',
        'url':'http://www.greenteapress.com/thinkpython/'},
        {'title':'Learn Python in 10 Minutes',
        'url':'http://www.korokithakis.net/tutorials/python/'} ]

    django_pages = [
```