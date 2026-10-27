
+   - The >= operator allows us to check whether the element we are comparing is greater than or equal to the comparing value:

```
{% if collection.all_products_count >= 25 %}
    The collection has more or an exact number of
    products as the comparing value!
{% endif %}
```

Since both values are the same data type, if the number of products is greater or equal, the logic will return true, and we will see our message shown. Otherwise, the logic returns false, and the message will not be visible.

+   - The <= operator allows us to check if the compared value is less than or equal to the comparing value:

```
{% if collection.all_products_count <= 25 %}
    The collection has less or an exact number of
    products as the comparing value!
{% endif %}
```

Similarly, as with our previous comparing element, we check whether the total number of products in our collection is less than or equal to the compared value. If we have met the condition, the logic returns true, and our message will show. Otherwise, the logic will return false, and the message will not be visible.

## Working with logic operators

Besides comparison operators, we also have access to two logic operators, which allow us to combine multiple conditions to create complex statements. We can divide them into the two following groups:

+   - The or operator allows us to set multiple conditionals, where we must meet at least one of them:

```
{% if collection.title == "Winter Shoes" or
collection.all_products_count > 25 %}
    The collection name is Winter Shoes, or the
    collection contains more than 25 products!
{% endif %}
```

In the preceding example, we check whether the name of our collection is equal to `Winter Shoes` or if the collection contains more than 25 products. If we have met at least one of these two conditions, the logic will return `true`, and our message will be shown. Otherwise, the logic will return `false`, and the message will not be visible.

+   - Similarly, the `and` operator allows us to set multiple conditions. However, for this operator to return `true`, all conditions must be met:

```
```liquid
{% if collection.title == "Winter Shoes" and
  collection.all_products_count > 25 %}
The collection name is Winter Shoes, and the
  collection contains more than 25 products!
{% endif %}
```
```

In this example, we are trying to see if both conditions have been met by checking whether the collection name is equal to `Winter Shoes` and the collection contains more than 25 products. If we have met both of these conditions, the logic will return `true`, and the message will be shown. Otherwise, the logic will return `false`, and the message will not be visible.

> **Important note:**
> 
> We can now create a conditional with multiple comparison values. However, we should note the order in which our conditional will perform the check is from the right side. We cannot change this order using the parentheses, as parentheses are invalid characters in Liquid and will break our code.

+   - `contains` is the final operator on our list. This particular operator works differently than the previous ones as it does not check whether a comparing value is equal to a compared-to value. Instead, it allows us to check whether a string contains a substring:

```
```liquid
{% if collection.title contains "Christmas" %}
Our Christmas collection is stocked and ready to go!
{% endif %}
```
```

In the previous block of code, we check whether `collection.title`, which returns a string, contains the word "Christmas", and if it does, it shows the message. Note that `contains` is case-sensitive and will only return `true` if the sub-string strictly matches the part of the string.

> **Important note:**
> We can use `contains` to search for the presence of a substring inside a string or even to check whether our string is a part of the array of strings. However, we cannot use it to check for the presence of an object inside the array of objects. We can only use `contains` to search for the presence of strings.

We have learned a lot about using logical and comparison operators. However, we only learned how to compare one value against another known value. So *how would we check if a compared value, for example a collection description, exists?* Since `collection.description` returns content with which we are unfamiliar, we have no way of performing this check with our current knowledge. To perform this action, we would first need to learn the different types of data we can use with Liquid.

## Understand the types of data

So far, we have mentioned two data types, **strings** and **numbers** to be precise. However, in Liquid, there are six different types of data that are available to us:

+   - Strings
+   - Number
+   - Boolean
+   - Nil
+   - Array
+   - EmptyDrop

### Strings

A **string** is a type of data that we use to represent text. Since a string can be any combination of letters, numbers, or special characters, we should always encapsulate it with quotation marks:

```
{% if product.title contains "Book" or product.title contains "2021" %}
    We have found a product that contains the word Book or
    the product that contains the word 2021.
{% endif %}
```

In the previous example, we check whether the product title contains the string "Book" or the same product title contains the string "2021", and if it does, our message will be shown.

### Number

A **number** is a type of data that require no quotation marks, and we use it to represent the following two types of numerical data:

+   - A **float** is a floating-point number, meaning that the number contains a decimal point.
+   - An **integer** or **int**, on the other hand, is a whole number without a decimal point:

```
{% if product.price > 25 and product.price < 3500 %}
    The number of products in a collection is greater
    than 25, and the product's price is lower than 3500.
{% endif %}
```

In this example, we check whether the product price is greater than 25, and at the same time, lower than 3500. If both conditionals are true, the message will be shown. Remember that since we used the and comparator, both conditionals must be true. If either one of them returns false, our message will not be shown.

Notice that neither of our two comparing values, 25 or 3500, are encapsulated inside the quotation marks. Otherwise, they would become strings, which, as we remember, we cannot compare against the whole number that product.price will return.

### Boolean

**Boolean** is a type of data that can either be true or false:

```
{% if customer.accepts_marketing == true %}
    The customer has signed up for our newsletter!
{% endif %}
```

As we can see from the example, similar to the number data type, Booleans do not use quotation marks. If customer.accepts_marketing has signed up for our newsletter, the object will be true, and our message will be visible.

As we recall, we can only use nil to check whether an element such as an object exists. It cannot check its content, but what exactly does that mean?

```
{% if collection.description %}
<h3>{{ collection.description }}</h3>
{% endif %}
```

In this example, we have wrapped our element within a conditional using the nil data type to check whether a collection.description instance exists. However, as we can see, this is not the result that we hoped for:

```
<h3></h3>
```

Since nil only checks whether an element exists, in our case, our conditional has found that collection.description, while empty, does exist. Remember that all values inside Liquid except false and nil are by default truthy, meaning that even empty strings are considered truthy. For this reason, even though our collection.description instance was empty and returned an empty string (since we used nil inside a conditional), the result was that the value was truthy. Therefore, the code inside the conditional is visible.

To resolve this problem, instead of checking whether the value exists, we should check whether our value is not blank:

```
{% if collection.description != blank %}
<h3>The collection description element will show only if its content is not empty.<h3>
{% endif %}
```

In the previous example, we check to see whether our collection description is not empty using the not equal parameter against blank. If the result of this conditional is that collection.description is not blank, the message we have defined will show. Otherwise, the conditional will return false and the message defined inside the conditional will not render.
