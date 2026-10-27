With this, we now have access to the gardening gloves product object. To access all images attached to a product, we will use the `product_object` variable as our object, followed by the `images` attribute, to recover the array of images for the specific product. Since we are dealing with an array, we will have to use the `for` tag to loop over them:

```
```liquid
{% assign product_object = all_products["gardening-gloves"] %}
{% for image_item in product_object.images %}
  <img src="{{ image_item | img_url }}"/>
{% endfor %}
```
```

We have now successfully extracted an array of product images, which we can use to create powerful galleries using various slider plugins. However, notice that the size of the images we have on our storefront is relatively small.

This is because Liquid always defaults to a size of 100x100 if we do not introduce the `size` parameter. Let's introduce the `size` parameter by limiting the image dimension to 300 px in width and 300 px in height:

```
```liquid
{% assign product_object = all_products["gardening-gloves"] %}
{% for image_item in product_object.images %}
  <img src="{{ image_item | img_url: "300x300" }}"/>
{% endfor %}
```
```

Notice that our images now have 300 px in width but only 200 px in height even though we have specified that we would like 300 px in height. This is because the `size` parameter can only limit the image size by decreasing its size to match the assigned values. It cannot change the image aspect, nor can it increase the size of the image beyond the original image size.

In the previous example, we have used "300x300" to limit our product images in both width and height. However, we can also limit only one side by using "300x" to set the image width to 300 px or "x300" to limit the height to 300 px. If we only specify one of the two values, Shopify will automatically calculate the dimension of the image while maintaining the image aspect.