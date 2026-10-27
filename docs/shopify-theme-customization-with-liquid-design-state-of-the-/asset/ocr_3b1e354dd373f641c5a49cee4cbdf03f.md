# Summary

In this chapter, we have learned how using something trivial such as a filter to manipulate different data types can create powerful features. We have learned how using URL and HTML filters can provide us with access to the various types of assets throughout Shopify and help us generate them in the storefront using their respective HTML tags.

Working on the product media gallery project has provided us with a deeper understanding of media objects and filters, which every developer needs to be familiar with. The product accordions project taught us how to easily manipulate data using string and array filters to create unique page content elements that are clean and easily maintainable. Moving on to the math and money filters, we have gained much-needed insight into performing complex calculations through Shopify and formatting the prices according to the currency formatting set in our store.

Lastly, we learned about the additional filters, which provided us with essential knowledge on how we can assign default value variables with no assigned values and make Liquid code readable by JavaScript. By understanding how to use the translation keys, we now have the necessary knowledge to quickly update any value across our theme without manually updating each value.

The knowledge that we have attained through this chapter will be especially helpful in the next chapter, where we will learn more about the JSON settings and how we can use them to create settings that merchants can access using the theme editor.

# Questions

+   1. Suppose that we have an array named product_handles with handles of 30 products. What issue in the following code would prevent us from outputting the images of all 30 products successfully?

```
{% for handle in product_handles %}
  {% assign product_object = all_products[handle] %}
  {% for image_item in product_object.images %}
    <img src="{{ image_item | img_url }}"/>
  {% endfor %}
{% endfor %}
```