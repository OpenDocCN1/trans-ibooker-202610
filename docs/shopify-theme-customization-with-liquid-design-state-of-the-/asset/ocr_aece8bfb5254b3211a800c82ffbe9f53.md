```
</a>
{% form "product", product %}
<input type="submit" value="Add to Cart"/>
{% endform %}
{% endif %}
```

With the addition of the **Add to Cart** button, we have created a button that should allow us to purchase the products directly from our collection page, without having to navigate to the product page. However, if we were to click it right now, we would encounter an error stating **Parameter Missing or Invalid: Required parameter missing or invalid: items:**

![](bbox=[0.202, 0.326, 0.772, 0.706])

Figure 4.1 – Result of submitting a product form with missing parameters

The parameter that we are missing is the `id` property of the variant that we are looking to purchase.