# Web form for orders: Coffee shop on the web

The next day Erik, Emily, and Simon got together to continue working on the coffee shop application.

"Are you ready to get serious?" Simon asked. "Today we have to work on our actual web form. We tried a very simple menu yesterday; now we have to add two more menus and the text field to enter customer’s name. Let’s look at our code from yesterday. We already created one menu there—the code between tags <select> and </select>. And this menu now has two options: coffee and decaf."

# Listing 11.1 First menu options in templates/forms.html

```
{% extends "base.html" %}
{% block content %}

<form action="/order" method="post">
    <select name="drink">
        <option value="">-- Choose drink --</option>
        <option value="coffee">Coffee</option>
        <option value="decaf">Decaf</option>
    </select>
    <input type="submit" value="Submit">
</form>
{% endblock %}
```

+   ① Menu option for coffee
+   ② Menu option for decaf

"Now we have to add another option to the menu drink—Erik, I think you had 'chocolate' in that menu, right?"

"Yes, coffee, chocolate, and decaf," Erik confirmed.

"Good. And what about flavors and toppings?"

"For flavors we have caramel, vanilla, peppermint, raspberry, and plain," Erik said. "For