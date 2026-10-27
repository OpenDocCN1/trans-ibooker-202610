Besides providing us with the ability to interpolate the variables, we can also escape the translated content, include the HTML in translation keys, and pluralize translation keys, making it a pretty powerful tool at our disposal.

> Tip:
> For additional information on the t filter naming and grouping guidelines, we can refer to https://shopify.dev/tutorials/develop-theme-localization-organize-translation-keys.
> For additional information on all the available arguments for the t filter, we can refer to https://shopify.dev/tutorials/develop-theme-localization-use-translation-keys.

Once we have created the necessary translation keys, we can also update the translation by navigating to our admin page and clicking on Themes located inside the expanded Online store option within the sidebar. Clicking on the Actions button on the theme file will trigger a dropdown, whereby clicking on Edit Language will quickly update any translation inside the language JSON file.

# The JSON filter

The JSON filter, as its name suggests, allows us to convert strings into JSON, and more importantly, will make Liquid code readable by JavaScript:

```
var product_JSON = {{ product | json }};
var cart_JSON = {{ cart | json }};
```

Note that when using the JSON filter on Liquid output, there is no need to include quotations marks, as the JSON filter will include them automatically. However, note that specific values, such as the inventory_quantity and inventory_policy fields, are not something that we can return via JSON, as Shopify has deprecated these fields due to security reasons.

> Tip:
> For additional information on all the available additional filters, we can refer to https://shopify.dev/docs/themes/liquid/reference/filters/additional-filters.