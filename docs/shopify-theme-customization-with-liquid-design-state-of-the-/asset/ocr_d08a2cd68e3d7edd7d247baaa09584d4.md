While we have not covered all the math and money filter types, we have gained a solid understanding of how both math and money filters work through this project. This type of knowledge will serve as a stepping stone for the features that we will be working on as Shopify experts.

# Exploring the additional filters

The **additional filters** are a set of filters that do not fit under any other filter groups. However, this does not make them any less important. While there are many types of filters that we can name here, we will only mention three of them that are the most essential as we will be using them regularly.

# The default filter

As its name suggests, the **default filter** allows us to set a default value for any variable, whether it is a string, array, or hash type. Note that we can only return the default value if the variable returns `nil`, `false`, or an empty string. If the variable contains whitespace characters, we will not be able to return the `default` value:

```
Hello {{ customer.name | default: "customer" }}
```

By introducing the `default` value in the previous example, we have ensured that we will not end up with a broken string even if the customer has not provided us with their name. Additionally, we also make our code look a lot cleaner. Without the `default` filter, we would have to use an `if` statement to check whether `customer.name` exists, and depending on the results, output the value.

# The t (translation) filter

The **t filter** is a translation key that allows us to access the currently active store language file under the `Locales` directory. If we navigate to the `Locales` directory, we will notice a large number of files. However, one of them will contain the string default, which is the currently active language in our store.

Let's look over one of our previous projects, `Custom collections`, which we worked on in *Chapter 4, Diving into Liquid Core with Objects*, in the *Working with global objects* section, where we developed a submit button whose value we hardcoded with the text **Add to Cart**. This works well for now, *but what if we changed our store language?* We would have to update any occurrence of the **Add to Cart** string manually for the entire theme.