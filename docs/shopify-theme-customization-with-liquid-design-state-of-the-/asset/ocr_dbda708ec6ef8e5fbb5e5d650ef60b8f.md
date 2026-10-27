# The textarea input

The textarea type of input works similarly to the text input, with the only difference being that textarea is a multi-line text field as compared to the text input, which is a single-line field. In addition to the standard attributes, we can also use a placeholder attribute to include a placeholder value for the textarea input:

```
{
  "type": "textarea",
  "id": "header_announcement_textarea",
  "label": "Header Announcement",
  "placeholder": "Enter a short announcement.",
  "default": "Spend more than 100$ to qualify for a 10% discount!"
}
```

Similarly, as the text input value, the textarea input value will always return a string value, unless it is empty, in which case it will return an EmptyDrop value. We can see the example of the textarea input in the following screenshot:

![](bbox=[0.32, 0.455, 0.63, 0.625])

Figure 6.10 – Example of the textarea basic input type

With textarea, we have now covered all of the basic input settings, which has set us on a solid path to understanding and working with theme editor .json files. However, to truly be able to say that we have a working knowledge of .json files, we will also need to learn about the specialized input settings as well.