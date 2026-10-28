Here’s the code for the Entry model. Place it in your *models.py* file:

# *models.py*

```
from django.db import models

class Topic(models.Model):
    --snip--

① class Entry(models.Model):
    """Something specific learned about a topic."""
②   topic = models.ForeignKey(Topic, on_delete=models.CASCADE)
③   text = models.TextField()
    date_added = models.DateTimeField(auto_now_add=True)

④   class Meta:
        verbose_name_plural = 'entries'

    def __str__(self):
        """Return a simple string representing the entry."""
⑤       return f"{self.text[:50]}..."
```

The Entry class inherits from Django’s base Model class, just as Topic did ①. The first attribute, topic, is a ForeignKey instance ②. A *foreign key* is a database term; it’s a reference to another record in the database. This is the code that connects each entry to a specific topic. Each topic is assigned a *key*, or ID, when it’s created. When Django needs to establish a connection between two pieces of data, it uses the keys associated with each piece of information. We’ll use these connections shortly to retrieve all the entries associated with a certain topic. The on_delete=models.CASCADE argument tells Django that when a topic is deleted, all the entries associated with that topic should be deleted as well. This is known as a *cascading delete*.

Next is an attribute called text, which is an instance of TextField ③. This kind of field doesn’t need a size limit,