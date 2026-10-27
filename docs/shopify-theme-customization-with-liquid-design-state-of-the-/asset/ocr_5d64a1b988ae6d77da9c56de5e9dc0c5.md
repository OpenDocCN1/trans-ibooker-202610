+   - The fourth and final additional attribute, `unit`, is an optional attribute, which allows us to set the visual unit, such as px, for the range slider value. Note that the `unit` attribute only accepts up to three characters and will output the px purely visually inside the theme editor. The actual value will return a number value without the unit.

One additional change to the standard set of attributes is that the `default` attribute is now mandatory:

```
```json
{
  "type": "range",
  "id": "logo_size",
  "min": 120,
  "max": 220,
  "step": 1,
  "unit": "px",
  "label": "Logo Size",
  "default": 140
}
```
```

Note that the `min`, `max`, `step`, and `default` attributes are a number type of value. Including a string value in any of these attributes will result in an error.

When compared to the previous attributes, the `range` attribute comes with a few rules that we must follow. The first rule is that the `default` value must be a value between the `min` and `max` values. The second and more important rule is that each range slider can have a maximum of 100 steps, but what exactly does this mean?

For example, in the previous example, we have set a min value of 120 and a max value of 220. Since we have set the `step` value to 1, we have precisely 100 steps between the two values.

On the other hand, if we set the max value to 320, we would also have to update the `unit` value to 2 to retain the 100 steps between the min and max value. In the following screenshot, we can see the example of a range input type:

![](bbox=[0.323, 0.777, 0.644, 0.88])

Figure 6.7 – Example of the range basic input type