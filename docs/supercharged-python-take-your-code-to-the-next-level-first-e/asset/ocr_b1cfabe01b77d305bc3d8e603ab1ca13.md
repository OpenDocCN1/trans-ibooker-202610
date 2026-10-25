# # 10.11 SETTING THE DEFAULT CURRENCY

To make our Money class friendlier to a wider group of people, we should enable users of the class to set default units other than U.S. dollars. An easy way to implement this feature is to tie it to a class variable and then let the user of the class change it as desired.

To do that, we first need to add a class variable to the Money class, a very easy change to make.

```
```python
class Money() :
    default_curr = 'USD'
```
```

Then we need to alter the `__init__` function. This is trickier than it sounds, because although you can refer to class variables from within a method definition, you can't use such a reference in the argument list. So the following causes an error:

Click here to view code image

```
```python
# This causes an ERROR!
def __init__(self, v='0',
             units=Money.default_curr):
```
```

It's frustrating that we can't do this. However, the following definition of the `__init__` function works perfectly well, by replacing the default value (an empty string) with the value stored in `default_curr`.