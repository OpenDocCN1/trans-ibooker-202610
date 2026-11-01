Also, the information contained in your classes should depend on your program's needs. Many OOP tutorials use a Car class as their basic example without noting that what goes into a class depends entirely on the kind of software you're writing. There's no such thing as a generic Car class that would obviously have a honkHorn() method or a numberOfCupholders attribute just because those are characteristics real-world cars have. Your program might be for a car dealership web app, a car racing video game, or a road traffic simulation. The car dealership web app's Car class might have milesPerGallon or manufacturersSuggestedRetailPrice attributes (just as a car dealership's spreadsheets might use these as columns). But the video game and road traffic simulation wouldn't have these attributes, because this information isn't relevant to them. The video game's Car class might have an explodeWithLargeFireball() method, but the car dealership and traffic simulation, hopefully, would not.

# Creating Objects from Classes

You've already used classes and objects in Python, even if you haven't created classes yourself. Consider the datetime module, which contains a class named date. Objects of the datetime.date class (also simply called datetime.date objects or date objects) represent a specific date. Enter the following in the interactive shell to create an object of the datetime.date class:

```
```
>>> import datetime
>>> birthday = datetime.date(1999, 10, 31) # Pass the year, month, and day.
>>> birthday.year
1999
>>> birthday.month
10
>>> birthday.day
31
>>> birthday.weekday() # weekday() is a method; note the parentheses.
6
```
```

Attributes are variables associated with objects. The call to datetime.date() creates a new date object, initialized with the arguments 1999, 10, 31 so the object represents the date October 31, 1999. We assign these arguments as the date class's year, month, and day attributes, which all date objects have.

With this information, the class's weekday() method can calculate the day of the week. In this example, it returns 6 for Sunday, because according to Python's online documentation, the return value of weekday() is an integer that starts at 0 for Monday and goes to 6 for Sunday. The documentation lists several other methods that objects of the date class have. Even though the date object contains multiple attributes and methods, it's still a single object that you can store in a variable, such as birthday in this example.