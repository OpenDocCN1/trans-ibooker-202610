# The offset parameter

The offset parameter allows us to delay the start of the for loop by starting the loop from the specific index. We can add an offset parameter at the end of the opening for loop tag, followed by a colon, followed by a number value.

Let's take our previous example and try replacing limit with the offset parameter and setting its value to 4:

```
{% for product in collections["outdoor"].products offset: 4 %}
{% if product.price > 10000 %}
{% continue %}
{% else %}
{{ product.title }}
{% endif %}
{% endfor %}
```

Our for loop has automatically skipped the first four product iterations and has started iterating the products at index number 5, resulting in our for loop returning three product iterations. However, what if we also include the limit parameter after the offset parameter and set its value to 1?

```
{% for product in collections["outdoor"].products offset: 4 limit: 1 %}
{% if product.price > 10000 %}
{% continue %}
{% else %}
{{ product.title }}
{% endif %}
{% endfor %}
```

While at first, it might look like the previous example will not work, this is a fully functional loop with valid parameters. As we have previously mentioned, the limit parameter allows us to limit a for loop to a specific number of iterations. With two parameters added, our for loop will start its iteration at index 5. It will cover one iteration as per the value set with the limit parameter, and then it will stop the for loop, regardless of whether the one iteration managed to get past our if statement.