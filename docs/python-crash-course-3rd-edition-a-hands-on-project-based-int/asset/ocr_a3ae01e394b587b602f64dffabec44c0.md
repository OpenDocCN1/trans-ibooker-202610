every class we’ve discussed. Here’s the model for the topics users will store:

```
from django.db import models

class Topic(models.Model):
    """A topic the user is learning about."""
    text = models.CharField(max_length=200)
    date_added = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        """Return a string representation of the model."""
        return self.text
```

We’ve created a class called Topic, which inherits from Model—a parent class included in Django that defines a model’s basic functionality. We add two attributes to the Topic class: text and date_added.

The text attribute is a CharField, a piece of data that’s made up of characters or text ①. You use CharField when you want to store a small amount of text, such as a name, a title, or a city. When we define a CharField attribute, we have to tell Django how much space it should reserve in the database. Here we give it a max_length of 200 characters, which should be enough to hold most topic names.

The date_added attribute is a DateTimeField, a piece of data that will record a date and time ②. We pass the argument auto_now_add=True, which tells Django to automatically set this attribute to the current date and time whenever the user creates a new topic.

It’s a good idea to tell Django how you want it to represent an instance of a model. If a model has a __str__() method, Django calls that method whenever it needs to generate output referring to an instance of that model. Here we’ve written a __str__() method that returns the value assigned to the text attribute ③.