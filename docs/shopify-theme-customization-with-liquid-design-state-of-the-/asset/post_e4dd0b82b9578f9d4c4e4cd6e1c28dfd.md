
When used inside the collection page, the preceding code will provide us with the data we are looking for. However, *what if we wanted to access the indoor collection object while visiting the outdoor collection page?*

This is where our knowledge of accessing the page object using the page handle, which we covered in *Chapter 2, The Basic Flow of Liquid*, in the *EmptyDrop* subsection of the *Understanding the types of data* section comes to help.

We can access the object using its handle by pluralizing the object's name we are trying to access, followed by either a squared bracket ( [ ] ) or dot ( . ) notation:

```
{% assign customCollection = collections["indoor"].title %}
```

```
{% assign customCollection = collections.indoor.title %}
```

By defining a variable with the object of the indoor collection, we can output the data for that collection on any page by simply invoking the object, followed by the attribute that we need:

```
{{ customCollection.title }}
```

While our object name differs from the original object name, it will allow us to output the same information compared to when we used {{ collection.title }} inside the collection template.

> **Important note:**
> In the previous example, we created a variable using the `customCollection` name. However, note that you can create this variable using any name of your choice, including the collection itself. While the `collection` keyword is not reserved, we should pay close attention when using the keywords already in use as this can cause unexpected results.

Considering that global objects is an extensive topic, it would not be very productive to explain each object and their attributes separately. Instead, we will be creating a few projects to see firsthand how to work with different kinds of objects and the data types they return.

As we have already mentioned, an object, in combination with attributes, allows us to read the information from our admin and dynamically output it to create various functionalities. Let's start by familiarizing ourselves with the collection and product objects, which we will use to finalize the Custom `collection` project that we started in the previous chapter.

## Custom collection

In the previous chapter, we created a for loop that outputs the names of the products from the outdoor collection, whose price is lower than $100.00. We did this by creating a collection-form snippet whose content we are outputting using the for parameter, combined with the render tag. We placed this at the bottom of our collection.liquid template file:

```
{% render "collection-form" for collections["outdoor"]
.products as product %}
```

Inside our snippet, we added an if statement to check if the product price is higher than 10000. If it is, we should output the product form tag and the product title:

```
{% if product.price > 10000 %}
{% continue %}
{% else %}
{% form "product", product, data-location: "collection" %}
{{ product.title }}
{% endform %}
{% endif %}
```

If we were to preview our code by visiting the indoor collection page, we would only see a list of names, so let's try to improve this snippet by writing some code that will output the entire product block instead of only the product's name.

Let's start by refactoring the code inside the snippet by removing the continue and capture tags, refining our statement by removing the else statement, wrapping {{ product.title }}, positioning it above the product form, and finally removing the extra parameters from our product form tag.

```
{% if product.price < 10000 %}
<p>{{ product.title }}</p>
{% form "product", product %}
{% endform %}
{% endif %}
```

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

```
</a>
{% form "product", product %}
<input type="submit" value="Add to Cart"/>
{% endform %}
{% endif %}
```

With the addition of the **Add to Cart** button, we have created a button that should allow us to purchase the products directly from our collection page, without having to navigate to the product page. However, if we were to click it right now, we would encounter an error stating **Parameter Missing or Invalid: Required parameter missing or invalid: items:**

![](img/4e10d2f6d65d6d1c551f760d91c2c116_117_0.png)

Figure 4.1 – Result of submitting a product form with missing parameters

The parameter that we are missing is the `id` property of the variant that we are looking to purchase.

Every product on Shopify can have up to three different sets of options. For example, a product can have multiple sizes, colors, and materials. Each combination of these three choices generates a unique number called a variant id, which tells our product form which combination of options it should place in the cart.

Note that these options are entirely optional as we can have a product without any variant options. However, even then, we still need to include the variant id. We should include this variant id as a value attribute of the hidden HTML input element with id as its name attribute:

```
{% if product.price < 10000 %}
  <a href="{{ product.url }}">
    <img src="{{ product | img_url }}"/>
    <p>{{ product.title }}</p>
    <p>{{ product.price | money }}</p>
  </a>
  {% form "product", product %}
    <input type="hidden" name="id" value="{{ product.first_available_variant.id }}" />
    <input type="submit" value="Add to Cart"/>
  {% endform %}
{% endif %}
```

With the addition of the variant id, we now have a fully functional product form. By clicking the Add to Cart button, we can submit the request to the Shopify servers, where the product and its variant will be identified based on the submitted variant id and added to our cart.

Notice that we will also be automatically redirected to the cart page after clicking the Add to Cart button. This is the default behavior that we can rectify in one of two ways.

The first way would require us to add the return_to parameter to our product form tag, which will allow us to set the page we should return to after submitting the form. We can remind ourselves of how to use the return_to parameter by returning to the previous chapter and revisiting The form tag subsection, located under the Theme tags section. The other way would be to use the Shopify Ajax API, which we will learn about later in this book.

Let's return to the indoor collection for now. Let's look at the entire collection page and compare the initial collection products with our custom collection at the bottom. You will notice that besides being styled in a much better way than our collection, the initial collection products also contain a red sale badge, along with a regular and discounted price.
