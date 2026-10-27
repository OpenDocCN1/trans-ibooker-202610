Note that we need to set the `default` attribute to one of the values previously defined inside the `options` array. Otherwise, if the `default` attribute is not defined, the first radio will be selected by default. Here is an example of the radio input type:

![](bbox=[0.281, 0.172, 0.719, 0.388])

Figure 6.6 – Example of the radio basic input type

Once we have the `radio` input defined, we can access it by pairing the `settings` keyword and the ID of the radio input:

```
```css
.heading {
    text-align: {{ settings.heading_alignment }};
}
```
```

The radio button value will always return a string value.

# The range input

With the range input type, we can create a range slider field. Compared to the previous inputs, the range input has four additional attributes and one change to the standard attributes. We can list the additional attributes in the following way:

+   - The `min` attribute allows us to set the minimum value of the range input. The `min` value is mandatory.
+   - The `max` value is also a mandatory attribute that allows us to set the maximum value of the range input.
+   - The `step` value allows us to set the increment value between the steps of the slider. The `step` slider is mandatory.