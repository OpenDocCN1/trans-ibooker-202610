Clicking the **Older versions** button will reveal the drop-down menu where we can see the date and time of all the changes that we have made within this particular file. Each date represents the last time that we pressed the **Save** button to save our changes. Selecting any option will automatically revert our code to that particular point.

After reverting our changes, we are ready to add the `form` tag to our `for` loop:

```
```liquid
{% for product in collections["outdoor"].products offset: 4 %}
  {% if product.price > 10000 %}
    {% continue %}
  {% else %}
    {% form "product", product %}
      {{ product.title }}
    {% endform %}
  {% endif %}
{% endfor %}
```
```

By introducing the `form` tag within our `for` loop, we have generated an HTML form tag that we can see by inspecting the string elements that we see on our `Indoor` collection page. While the form tag is functional, we still lack one key ingredient to make our form usable, which we will be learning in the next chapter.

We can see that our `form` tag already has quite a few attributes by inspecting our collection page. *However, what if we wanted to add some of our own or modify the existing ones?* We can do this by simply adding the attribute name, followed by a comma, followed by the attribute's value:

```
```liquid
{% for product in collections["outdoor"].products offset: 4 %}
  {% if product.price > 10000 %}
    {% continue %}
  {% else %}
    {% capture productId %}productId-{% increment counter %}{% endcapture %}
    {% form "product", product, id: productId, data-location: "collection" %}
      {{ product.title }}
    {% endform %}
  {% endif %}
{% endfor %}
```
```