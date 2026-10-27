```
{% endfor %}
{% endfor %}
{% endfor %}
```

We now have all the necessary elements to create a working on-hover navigation menu. The only thing left is to add some HTML tags and apply the necessary styling to create the on-hover dropdown effect. However, before we proceed with styling, let's try to be a bit more creative.

Notice that our menu links are just that – links. But what if we wanted to be more creative by showing an image with every product menu item? For this to work, we would need to identify which menu items are pointing toward the products, which we can do by using the type attribute and comparing whether the returned value is equal to the product_link string. Since we already know that only the third level of the navigation contains the product menu items, we will only include this feature inside the third for loop:

```
{% assign collection-menu = linklists.indoor-navigation %}
{% for link in collection-menu.links %}
{{ link.title }}
{% for sub_link in link.links %}
{{ sub_link.title }}
{% for sub_sub_link in sub_link.links %}
{% if sub_sub_link.type == "product_link" %}
<img src="{{ sub_sub_link | img_url: "250x250" }}"/>
{% endif %}
{{ sub_sub_link.title }}
{% endfor %}
{% endfor %}
{% endfor %}
```

Now, it makes sense that this should work and that, as a result, we should be able to see four images of our products inside the navigation menu. However, note that sub_sub_link is still only a link object, and the link object does not have an image attached to it. To show the image that's attached to the product, we will have to recover the object of the product that our link is pointing to.