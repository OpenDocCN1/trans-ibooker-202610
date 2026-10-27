# ## Question 4

What adjustment do we need to make to the input element if we were looking to capture the `line_item` value and hide it on the checkout page?

```
```html
<input type="text" name="properties[Your Name]"
placeholder="Your Name"/>
```
```

# ## Answer

If we were looking to capture the `line_item` value, we would need to introduce an underscore as a first character inside the square bracket. By introducing this underscore within the `line_item` input, we will automatically hide the specific `line_item` from the checkout page. However, this will not hide it on the cart page. The cart page will require some manual adjustments. We can remind ourselves of how this works by going back to the *Custom navigation* subtopic, in the *Working with global objects* section.

# # Chapter 5, Diving into Liquid Core with Filters

# ## Question 1

Suppose that we have an array named `product_handles` with handles of 30 products. What issue in the following code would prevent us from outputting the images of all 30 products successfully?

```
```liquid
{% for handle in product_handles %}
  {% assign product_object = all_products[handle] %}
  {% for image_item in product_object.images %}
    <img src="{{ image_item | img_url }}"/>
  {% endfor %}
{% endfor %}
```
```

# ## Answer

Since we are looking to output more than 20 products, in this case, 30 products, we cannot use the `all_products` object, as the `all_products` object has a limitation that we can only call it 20 times on a single page. If we are looking to recover data from more than 20 products, we need to assign them to a collection and then perform a loop over those products. We can remind ourselves of the `all_products` object by visiting the *Working with HTML and URL filters* section.