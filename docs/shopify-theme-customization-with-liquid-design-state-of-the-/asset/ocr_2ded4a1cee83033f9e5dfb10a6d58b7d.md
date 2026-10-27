```
<p>Lorem ipsum content<p>
</div>
<div class="product-single__description-item">
<span>Instructions</span>
<p>Lorem ipsum content<p>
</div>
```

Looking over the results, we can see that we were successful in extracting each product description block. However, there are still a few things to cover. For example, we will have one empty div, which we can quickly resolve by introducing the offset parameter and setting its value to 1, allowing us to skip the first iteration within the for loop. We can remind ourselves of the offset parameter by visiting the Iterations tags section located in Chapter 3, Diving into Liquid Core with Tags:

```
<div class="product-single__description rte">
{% assign productDescription = product.description | split: "<h6>" %}
{% for item in productDescription offset: 1 %}
<div class="product-single__description-item">
{{ item }}
</div>
{% endfor %}
</div>
```

Adding the offset parameter to the for tag will skip the first iteration, otherwise returning an empty value. Additionally, we have ensured that our array only contains the three product description blocks we initially intended. However, let’s take a closer look at the current results of our array after applying the offset parameter:

```
<div class="product-single__description-item">
<span>Description</span>
<p>Lorem ipsum content<p>
</div>
<div class="product-single__description-item">
<span>Ingredients</span>
<p>Lorem ipsum content<p>
</div>
<div class="product-single__description-item">
```