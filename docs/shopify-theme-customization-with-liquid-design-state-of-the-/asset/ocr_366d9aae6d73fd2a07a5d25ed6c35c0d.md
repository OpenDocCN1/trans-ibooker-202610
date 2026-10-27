# The `/products/{product-handle}.js` endpoint

The `/products/{product-handle}.js` endpoint is a simple endpoint that we can use in combination with a GET request to retrieve information about any product in the store easily. Similarly, as with the `/cart.js` endpoint, `/products/{product-handle}.js` is relatively easy to use as it only requires us to include the handle of a product we are looking to retrieve data about:

```
const getProduct = (handle) => {
  fetch(`/products/${handle}.js`)
    .then(response => response.json())
    .then(product => {
      console.log(product.id);
    });
}
```

The return value that we will receive from the preceding example will include the product ID, which we will be using in the following example.

The most common use for this endpoint is when creating on-click functionalities, such as the quick view feature, where we need to load a lot of product information dynamically to avoid cluttering the DOM and slowing down the store.

# The `/recommendations/products.json` endpoint

The `/recommendations/products.json` endpoint allows us to retrieve a list of JSON objects regarding the recommended products for the selected product based on Shopify algorithms, which we can use to construct a dynamic recommended section.

Through this endpoint, we can use three parameters, one of which is mandatory, while the other two are optional:

+   - The `product_id` parameter is a mandatory parameter whose value should be set to the ID of the product whose recommendation list we are looking to retrieve. Note that the product ID is not the same as the variant ID. They are two different attributes that we can retrieve through the `product` object.
+   - The `limit` parameter is an optional parameter that allows us to select the maximum number of recommended products we should receive per request. We cannot retrieve more than 10 recommended products per request due to Shopify limitations. This is the default value if we do not set the `limit` parameter.