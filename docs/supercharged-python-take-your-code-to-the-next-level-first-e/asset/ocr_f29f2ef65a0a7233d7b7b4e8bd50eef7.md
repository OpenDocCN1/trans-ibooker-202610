At this point, `name`, `breed`, and `age` are all attributes of the `my_dog` object only. If you create some other Dog objects, they will not necessarily have these same attributes (that is, instance variables). However, we can choose to attach these same attributes to another Dog object whenever we want.

```
yr_dog = Dog()
top_dog = Dog()
hot_dog = Dog()
hot_dog.name = 'Hotty Totty'
hot_dog.breed = 'Dachshund'
```

How do you ensure that every object of the same class has these attributes (instance variables)? The answer is that’s what the `__init__` method is for!

# ## 9.3 THE "`__INIT__`" AND "`__NEW__`" METHODS

For each class, the `__init__` method, if defined, is automatically invoked whenever an object of the class is created. You can use this method to make sure every instance of the class supports the same common set of variables ... but each will have its own values.

![](bbox=[0.146, 0.765, 0.255, 0.844])

Click here to view code image