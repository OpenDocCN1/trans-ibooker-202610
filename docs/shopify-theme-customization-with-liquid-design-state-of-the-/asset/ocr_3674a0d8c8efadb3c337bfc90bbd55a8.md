Calling our three variables will generate the same type of data that we have initially assigned to each variable:

```
"This is a string!"
2021
true
```

Note that calling a variable using the double curly brace delimiters will return the variable value when called on its own. If we wanted to use a variable within a for tag or an if statement, we would call the variable only using its name, without the double curly brace delimiters:

```
{% if stringVar == "This is a String!" %}
    The variable content is a string value!
{% elsif stringVar == 2021 %}
    The variable content is a number value!
{% else %}
    The variable content is a boolean value!
{% endif %}
```

We have used our previously declared variable inside a multiple if statement to determine our variable's value. Since our variable value is equal to the compared value of the first statement, the first statement will return true.

So far, we have learned how to assign a single type of data to a variable, but what if we wanted to create a variable that will store a combination of a string and a variable? To achieve this, we will need the help of another type of variable tag called capture.

# The capture tag

As opposed to the assign tag, which only allowed us to capture a single value, capture allows us to capture multiple values using its start and end closing tags. We can declare a capture variable with a set of curly brace delimiters with percentage symbols around the word capture followed by the name of the variable we are declaring:

```
{% assign percent = 15 %}
{% assign deal = "three or more" %}
{% capture promoMessage %}
    If you order {{ deal }} books, you can receive up to a {{ percent }}% discount!
```