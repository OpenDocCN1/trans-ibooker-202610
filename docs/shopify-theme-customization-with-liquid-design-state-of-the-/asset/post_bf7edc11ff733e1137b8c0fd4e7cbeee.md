
With the current setup, we display both the regular price and the comparison price, so let's start by modifying the comparison price and replacing it with the actual discount price. To output the discount between the two prices, it should be relatively straightforward as we only need to subtract the regular price from the comparison price.

To subtract the regular price from the comparison price, we will need to use the minus math filter:

```
<p class="custom-collection--price">
  {{ product.price | money }}
  <span>{{ product.compare_at_price | minus: product.price }}</span>
</p>
```

Notice that with the introduction of the minus filter, we had to remove the money filter entirely. As previously mentioned, we can only use math filters if all the values are number values. If we were to include money filters, we would turn both values into a string due to our previously set currency formatting.

Now that we have successfully applied the minus filter, we need to think of a way to include the money filter. With the current setup, we have received a number value without any currency formatting. However, as mentioned, if we were to apply the money filter to any of the two values, even after product.price, the money filter would only affect product.price, not the final results. Consequently, the math filter would no longer work.

To resolve this, we would need to introduce a variable using an assign or capture keywords to calculate the difference between the two numbers and later call the variable with the money filter:

```
<p class="custom-collection--price">
  {{ product.price | money }}
  {% assign discount-price = product.compare_at_price | minus: product.price %}
  <span>{{ discount-price | money }}</span>
</p>
```

If we preview our collection page now, we will see that we have correctly performed both the discount price calculations and the currency formatting. So now we can move to the second part of our project, which is replacing the sale badge with an actual percentage value discount.

We can find the sale badge HTML code within the same collection-form.liquid snippet file:

```
{% if product.compare_at_price != blank %}
<div class="custom-collection--item">
  <a href="{{ product.url }}">
    <img src="{{ product | img_url: "300x300" }}"/>
    <p class="custom-collection--price">
      {{ product.price | money }}
      {% assign discount-price = product.compare_at_price | minus: product.price %}
      <span>Save {{ discount-price | money }}</span>
    </p>
    <span class="custom-collection--sale-badge">sale</span>
  </a>
  {% form "product", product %}
    <input type="hidden" name="id" value="{{ product.first_available_variant.id }}" />
    <input type="submit" value="Add to Cart"/>
  {% endform %}
</div>
{% endif %}
```

To calculate the discounted percentage, we can use the following formula, which will return the discounted percentage:

```
{{ product.compare_at_price | minus: product.price | times: 100 | divided_by: product.compare_at_price }}
```

In the previous example, we had to include the calculation inside a variable so that later we could apply the money filter. However, since this time we do not need money, we can simply include the calculation with the addition of the percentage string at the end. Let's put it all together now:

```
{% if product.compare_at_price != blank %}
<div class="custom-collection--item">
  <a href="{{ product.url }}">
    <img src="{{ product | img_url: "300x300" }}"/>
    <p class="h4 custom-collection--title">{{ product.title }}</p>
```

```
```liquid
<p class="custom-collection--price">
  {{ product.price | money }}
  {% assign discount-price = product.compare_at_price |
    minus: product.price %}
  <span>Save {{ discount-price | money }}</span>
</p>
<span class="custom-collection--sale-badge">{{
    product.compare_at_price | minus: product.price |
    times: 100 | divided_by: product.compare_at_price }}%</span>
</a>
{% form "product", product %}
  <input type="hidden" name="id" value="{{
    product.first_available_variant.id }}" />
  <input type="submit" value="Add to Cart"/>
{% endform %}
</div>
{% endif %}
```
```

As we had the chance to see, similarly as with the string filters, we could chain multiple math filters easily to perform the calculations we needed. However, it is worth mentioning that besides only working with number values, math filters do not accept any type of brackets, which we would usually use to perform math calculations in specific priority.

If we need to perform calculations, we have two choices. The first one is that we let Liquid perform math calculations from left to right. On the other hand, if we need to perform calculations in a specific order, we will need to split the formula into multiple variables and combine the results later.

> **Tip:**
> 
> For additional information on all the available math filters, we can refer to  
> https://shopify.dev/docs/themes/liquid/reference/filters/math-filters.
> 
> For additional information on all the available money filters, we can refer to  
> https://shopify.dev/docs/themes/liquid/reference/filters/money-filters.

While we have not covered all the math and money filter types, we have gained a solid understanding of how both math and money filters work through this project. This type of knowledge will serve as a stepping stone for the features that we will be working on as Shopify experts.

## Exploring the additional filters

The **additional filters** are a set of filters that do not fit under any other filter groups. However, this does not make them any less important. While there are many types of filters that we can name here, we will only mention three of them that are the most essential as we will be using them regularly.

## The default filter

As its name suggests, the **default filter** allows us to set a default value for any variable, whether it is a string, array, or hash type. Note that we can only return the default value if the variable returns `nil`, `false`, or an empty string. If the variable contains whitespace characters, we will not be able to return the `default` value:

```
Hello {{ customer.name | default: "customer" }}
```

By introducing the `default` value in the previous example, we have ensured that we will not end up with a broken string even if the customer has not provided us with their name. Additionally, we also make our code look a lot cleaner. Without the `default` filter, we would have to use an `if` statement to check whether `customer.name` exists, and depending on the results, output the value.

## The t (translation) filter

The **t filter** is a translation key that allows us to access the currently active store language file under the `Locales` directory. If we navigate to the `Locales` directory, we will notice a large number of files. However, one of them will contain the string default, which is the currently active language in our store.

Let's look over one of our previous projects, `Custom collections`, which we worked on in *Chapter 4, Diving into Liquid Core with Objects*, in the *Working with global objects* section, where we developed a submit button whose value we hardcoded with the text **Add to Cart**. This works well for now, *but what if we changed our store language?* We would have to update any occurrence of the **Add to Cart** string manually for the entire theme.

Using the t filter, we can update any string value through our entire theme by updating a single value. The first thing that we need to do is to define the three-level JSON inside the currently active language file using the Shopify naming and grouping guidelines. Since we are looking to modify the submit button of a product, we can set the first level to product, the second to something more specific, which is the form itself, and finally, the third level points to the string we are looking to translate:

```
```json
{
  "product": {
    "form": {
      "submit": "Add to Cart"
    }
  }
}
```
```

Once we have successfully created the JSON inside the language file, all that is left is to use the t filter to read the translated key value and render it on the storefront:

```
```liquid
{{ product.form.submit | t }}
```
```

By implementing a t filter on all product forms, we will gain the ability to automatically translate all the strings without having to search for them throughout our files manually. Additionally, using the same JSON naming and grouping inside the other language files will allow us to quickly translate our entire theme by simply changing the store language.

The t filter is a powerful tool that allows us to pass multiple arguments by separating them with commas and interpolating them:

```
```liquid
{{ header.general.customer | t: Customer: Customer.name }}
```
```

In the previous example, we are trying to access the customer string inside the language file. However, we have also passed the customer.name value as an argument, which we have then interpolated inside the language JSON file:

```
```json
{
  "header": {
    "general": {
      "customer": "Welcome {{ customer }}!"
    }
  }
}
```
```
