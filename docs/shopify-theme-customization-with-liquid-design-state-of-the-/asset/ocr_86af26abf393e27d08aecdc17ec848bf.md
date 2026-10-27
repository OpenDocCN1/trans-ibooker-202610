As we can see, as with the .json files in the Locales directory mentioned in the previous chapter, the settings_schema.json file has a specific format that we must honor. Using the name attribute, we can set the name of the category and the settings attribute will contain the array of settings that the category will contain:

```
{
  "name": "Category",
  "settings": [
    {
      "type": "color",
      "id": "store_background_color",
      "label": "Background Color",
      "default": "#ffffff"
    },
    {
      "type": "color",
      "id": "store_border_color",
      "label": "Border Color",
      "default": "#cccccc"
    }
  ]
}
```

In the previous example, we have included two types of color settings, one that will control the background color of our store and one that will be in charge of setting the border color we will use across our store. As we can see, we have enclosed each settings option within the curly brackets and separated it by commas. However, note that the last settings option inside the category does not have a comma.

> **Important note:**
> Including a comma after the last attribute within the settings block, or including the comma after the last settings block in the category, will result in an error and we will not be able to save our work.