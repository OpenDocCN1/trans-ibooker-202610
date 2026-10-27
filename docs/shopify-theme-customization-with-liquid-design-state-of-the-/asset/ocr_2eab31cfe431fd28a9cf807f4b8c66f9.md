To solve this, we can wrap {{ page.content }} along with the div that encapsulates it within a conditional that will check whether the content of the page is empty:

```
{% if page.content != blank %}
<div class="rte">
  {{ page.content }}
</div>
{% endif %}
```

We are now sure that we will not see any extra empty strings and elements on our page with the conditional in place.

Let's now try to include some kind of placeholder text instead and let anyone who visits our page know that we will add the page content shortly. This is where the else and elsif statements come to help us. With the addition of else and elsif, we can create multiple conditions to ensure that we execute our code correctly:

+   1. Let's start by adding the else condition before {% endif %} to execute a different result when our {{ page.content }} is empty:

```
{% if page.content != blank %}
<div class="rte">
  {{ page.content }}
</div>
{% else %}
<div class="rte">
  The page content is currently pending. Please check again shortly.
</div>
{% endif %}
```