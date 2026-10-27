Our product block is starting to look a lot better, but we still need to show the price of our products. We can include the product price by wrapping {{ product.price | money }} inside the p HTML tags:

```
{% if product.price < 10000 %}
<a href="{{ product.url }}">
<img src="{{ product | img_url }}"/>
<p>{{ product.title }}</p>
<p>{{ product.price | money }}</p>
</a>
{% form "product", product %}
{% endform %}
{% endif %}
```

> Tip:
> Notice that we have a small addition separated by a pipe character for both the image and price objects. This addition is called a filter, which helps us modify the output that we would otherwise receive from the object.

For example, if we were to call the product object with the price attribute, we would receive a number value without any format, such as 2599. However, by applying the money filter to the object, we have automatically changed the otherwise meaningless number into a string data type and formatted it according to our store-selected currency formatting, resulting in a $25.99 string.

We will not go into too much detail regarding filters right now, as we will be learning about them in the next chapter. For now, this basic information about filters will have to suffice. Let’s return to our example.

So far, we have included the product name, image, and price, which check most of the necessary boxes that are needed to present a product. However, notice that the product form is currently empty. Let’s change this by introducing an input element so that it’s of the submit type, which should allow us to purchase the product directly from the collection page:

```
{% if product.price < 10000 %}
<a href="{{ product.url }}">
<img src="{{ product | img_url }}"/>
<p>{{ product.title }}</p>
<p>{{ product.price | money }}</p>
```