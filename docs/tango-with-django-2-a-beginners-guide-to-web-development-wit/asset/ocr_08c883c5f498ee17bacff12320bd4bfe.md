```
# Import the Category model from the Rango application
>>> from rango.models import Category

# Show all the current categories
>>> print(Category.objects.all())
# Since no categories have been defined we get an empty QuerySet object.
<QuerySet []>

# Create a new category object, and save it to the database.
>>> c = Category(name='Test')
>>> c.save()

# Now list all the category objects stored once more.
>>> print(Category.objects.all())
# You'll now see a 'Test' category.
<QuerySet [<Category: Test>]

# Quit the Django shell.
>>> quit()
```

In the example, we first import the model that we want to manipulate. We then print out all the existing categories. As our underlying Category table is empty, an empty list is returned. Then we create and save a Category, before printing out all the categories again. This second print then shows the new Category just added. Note the name Test appears in the second print – this is the output of the __str__(), and neatly demonstrates why including these methods is important!

# Complete the Official Tutorial

The example above is only a very basic taster on database related activities you can perform in the Django shell. If you have not done so already, it’s now a good time to complete part two of the official Django Tutorial to learn more about interacting with models¹⁷. In addition, have a look at the official Django documentation on the list of available commands¹⁸ for working with models.

¹⁷https://docs.djangoproject.com/en/2.1/intro/tutorial02/
¹⁸https://docs.djangoproject.com/en/2.1/ref/django-admin/#available-commands