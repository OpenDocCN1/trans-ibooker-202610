+   - Last but not least is the `section_id` parameter, which, while optional, is quite an interesting parameter as it allows us to change the type of response that we will receive. By including the ID of a section as the `section_id` parameter value, we can select the parent element where we would like to render the recommended products. More importantly, we can also change the JSON response to an HTML string, which we can then use in combination with the `recommendations` object to output the recommended products dynamically.

Now that we have familiarized ourselves with all the attributes that we can use with the `/recommendations/products.json` endpoint, it is time to see them in action.

In the following example, we have used a `fetch` request, paired with the `/recommendations/products.json` endpoint, to generate a JSON object list and output them in the console log:

```
const productRecommendations = (productId, limit) => {
  fetch(`/recommendations/products.json?product_id=${productId}&limit=${limit}`)
    .then(response => response.json())
    .then(products => {
      console.log(products);
    });
}
```

As we can see, retrieving a JSON object for the recommended products is quite simple, as the only thing that we need to do now is pass the product ID to the `productId` parameter. As you may recall, the `limit` parameter is optional and will default to the maximum value of 10 when not included. Now, let's look at how we can include `section_id` and learn how to retrieve HTML strings instead.

Before we can modify the `fetch` request to accomplish this, we need to make specific preparations. The first thing we need to do is create a new section in the `Sections` directory. For our example, we will name it `recommended-products`.

Since we already have a recommended products section on the product page template and are only creating a new section to learn how this works, let's include this new section at the bottom of the `theme.liquid` layout file, just above the `</body>` tag. Now that we have created the section file and successfully included it, we must familiarize ourselves with the `recommendations` object.