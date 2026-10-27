+   6. After defining all the inputs inside the `blocks` module's object settings, we can click on the arrow on the left-hand side of the **Announcement** title to return to the previous page, where we can include additional `blocks` modules. However, note that since we have introduced a `limit` attribute with a value of 3, we can only repeat the **Announcement** block up to 3 times, as shown here:

![](bbox=[0.32, 0.22, 0.66, 0.48])

Figure 7.5 – Example of a limited number of section block modules

Once we have included the maximum number of modules, as per the defined value, the **Add Announcement** button will turn gray, signaling that we have reached the maximum number of `blocks` modules. Similarly, as with the dynamic sections, hovering over the `blocks` modules will reveal two icons. These will allow us to hide the currently selected block or rearrange the block's order using the drag-and-drop feature.

Now that we have learned how to create and use a `blocks` module, we need to learn how to output the values of the `blocks` module's input settings:

+   1. We can access the `blocks` module's object settings similar to how we accessed the `section` object: we will need to use a `section` object in combination with the `blocks` attribute. This combination of the `section` object and the `blocks` attribute will return an array of sections blocks that we can access using a simple for loop:

```
{% for block in section.blocks %}
{% endfor %}
```