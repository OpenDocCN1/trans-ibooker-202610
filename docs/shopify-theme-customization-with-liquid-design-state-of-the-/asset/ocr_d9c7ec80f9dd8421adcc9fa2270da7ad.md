As we have mentioned previously, most of the input settings will contain only the standard attributes. However, some of the specialized inputs and even basic inputs will require additional attributes. Let's now look into each input type, learn how to use it, and what type of results we can expect.

# The checkbox input

The checkbox type of input, as its name suggests, is a Boolean type of field that allows us to create a checkbox option within the theme editor:

```
```json
{
  "type": "checkbox",
  "id": "enable_popup",
  "label": "Enable popup",
  "default": true
}
```
```

As we can see, the checkbox input contains three mandatory and one optional attribute whose value we have set to true. Otherwise, if we were to remove the `default` attribute, the default state of the checkbox will be `false`. The following screenshot shows us an example of the checkbox input type:

![](bbox=[0.289, 0.52, 0.678, 0.623])

Figure 6.4 – Example of the checkbox basic input type

We can use the Boolean input type to toggle features on and off, which we can achieve by taking the checkbox input value and checking its current state using an `if` statement:

```
```liquid
{% if settings.enable_popup == true %}
{% endif %}
```
```

By checking whether the checkbox input value is equal to true, we have created a simple feature that will enable or disable a certain feature from the store easily, as the code block inside the statement will only render if the statement is true:

```
```liquid
{% if settings.enable_popup != blank %}
{% endif %}
```
```