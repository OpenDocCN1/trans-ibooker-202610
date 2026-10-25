![](bbox=[0.148, 0.088, 0.254, 0.168])

Click here to view code image

```
{ key1: value1, key2: value2, ... }
```

In plain English, a dictionary is like a flat, two-column table in a database. It lacks the advanced features of modern database management systems; it’s only a table. But it can still serve as a rich data-storage object in your Python programs.

The keys for a dictionary are a series of unique values; keys cannot be duplicated. For each key there’s an associated data object, called a value. For example, you can create a dictionary for grading a class of students as follows:

Click here to view code image

```
grade_dict = { 'Bob':3.9, 'Sue':3.9, 'Dick':2.5 }
```

This statement creates a dictionary with three entries—the strings “Bob,” “Sue,” and “Dick”—which have the associated values 3.9, 3.9, and 2.5, respectively. Note it’s perfectly fine to duplicate the value 3.9, because it’s not a key.

As usual, grade_dict is only a name, and you can give a dictionary almost any name you want (as long as the name obeys the rules listed earlier). I’ve chosen the name grade_dict, because it is suggestive of what this object is.

After a dictionary is created, you can always add a value through a statement such as this: