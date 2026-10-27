> **Important note:**
> We can use `contains` to search for the presence of a substring inside a string or even to check whether our string is a part of the array of strings. However, we cannot use it to check for the presence of an object inside the array of objects. We can only use `contains` to search for the presence of strings.

We have learned a lot about using logical and comparison operators. However, we only learned how to compare one value against another known value. So *how would we check if a compared value, for example a collection description, exists?* Since `collection.description` returns content with which we are unfamiliar, we have no way of performing this check with our current knowledge. To perform this action, we would first need to learn the different types of data we can use with Liquid.

# Understand the types of data

So far, we have mentioned two data types, **strings** and **numbers** to be precise. However, in Liquid, there are six different types of data that are available to us:

+   - Strings
+   - Number
+   - Boolean
+   - Nil
+   - Array
+   - EmptyDrop

# Strings

A **string** is a type of data that we use to represent text. Since a string can be any combination of letters, numbers, or special characters, we should always encapsulate it with quotation marks:

```
{% if product.title contains "Book" or product.title contains "2021" %}
    We have found a product that contains the word Book or
    the product that contains the word 2021.
{% endif %}
```