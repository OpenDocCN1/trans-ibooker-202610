
## Configuring the Theme Settings

In previous chapters, we have been learning about Shopify as a platform, familiarizing ourselves with Liquid basics, and using the Liquid core to create various features on the storefront. However, unless the store owners are developers, they will not have much control over any features we create for them.

In this chapter, we will be learning how we can use JSON to create settings that are accessible through the theme editor, which will allow the store owners to easily customize the theme without making the code adjustments throughout the theme. We will cover the following topics in this chapter:

- Exploring JSON settings
- Learning about the input setting attributes
- Basic and specialized input types
- Organizing the theme editor
- Glancing at the deprecated settings

By the time we complete this chapter, we will have gained a deeper understanding of the importance of JSON, and how we can use it to create theme settings that are accessible across the entire theme on any page. We can use these settings to modify the CSS values, change the content on certain features, and even use the settings to enable or disable a particular feature altogether. By learning how to use JSON to create these settings, we will take another step toward creating a genuinely dynamic and customizable feature storefront, which is what Shopify is.

## Technical requirements

While we will explain each topic and have it presented with the accompanying graphics, we will need an internet connection to follow the steps outlined in this chapter, considering that Shopify is a hosted service.

The code for this chapter is available on GitHub at https://github.com/PacktPublishing/Shopify-Theme-Customization-with-Liquid/tree/main/Chapter06.

The Code in Action video for the chapter can be found here: https://bit.ly/3nLQgMf

## Exploring JSON settings

In Chapter 1, Getting Started with Shopify, we briefly mentioned the Config directory, where we can define and manage the global JSON values across the entire theme. Let's remind ourselves of the two essential .json files that we can find within this directory:

- The `settings_schema.json` file allows us to create and manage the content inside the theme editor on our theme, which we can reference throughout the entire theme file.
- The `settings_data.json` file, on the other hand, records all the options defined in our schema file and saves their values. We can consider this file as our theme database, which we can manage by updating the theme settings through the theme editor or by directly editing the values inside the `settings_data.json` file.

We can group the global settings options into different categories for more straightforward navigation, which we can do using the name and settings attributes:

```
{
  "name": "Category",
  "settings": [
  ]
}
```

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

Now that we have defined the global settings, we need to learn how to access them and recover their values. We can recover the value of any global input setting using the settings keyword and the input's ID, whose value we are looking to recover, separated by a dot and encapsulated by double curly braces:

```
{{ settings.store_background_color }}
```

```
{{ settings.store_border_color }}
```

We have now had the chance to see how we can define input setting options and read their values, but where exactly are we rendering this option and how can we modify it?

We can access the theme editor by navigating to the Online theme section in the admin section and clicking on the Customize button on the theme we are looking to customize:

![](img/4e10d2f6d65d6d1c551f760d91c2c116_198_0.png)

Figure 6.1 – Example of accessing the theme editor through the Online theme section

Note that changes made throughout the theme editor are theme-specific, so we should remember to click the Customize button on the theme we are looking to customize.

Alternatively, we can access the theme editor through the code editor by clicking on the **Customize theme** button, located in the top-right corner:

![](img/4e10d2f6d65d6d1c551f760d91c2c116_199_0.png)

Figure 6.2 – Alternative way of accessing the theme editor through the code editor

Once inside the theme editor, we will see a list of category options within our sidebar. However, most of these options are a part of the sections and blocks that we will be learning more about in the following chapter. For now, we can access the global settings that we define throughout the settings_schema.json file by clicking on the **Theme settings** button located in the bottom-right corner:

![](img/4e10d2f6d65d6d1c551f760d91c2c116_199_1.png)

Figure 6.3 – Accessing the global settings inside the theme editor

Inside **Theme settings**, we will find all the categories and input settings that we have defined in the `settings_schema.json` file, where each input set contains a set of attributes. While some of them are required, others are optional. Let's learn more about them.

## Learning about the input setting attributes

Each input settings option can contain the following five attributes, which are also called standard attributes:

- As its name indicates, the `type` attribute allows us to set the type of the input setting, which can be either a basic type or a specialized type. The `type` attribute is mandatory.
- The `id` attribute is another mandatory attribute that we will use later to access and read the setting value.
- The `label` attribute allows us to set the label of the input settings inside the theme editor. The `label` attribute is mandatory.
- The `default` value serves as a failsafe and allows us to set a default option for the input settings. However, it is not mandatory.
- The final attribute, `info`, allows us to include an additional clarification regarding the input settings and is also not mandatory.

While most of the input settings will contain only the previously mentioned attributes, depending on the input type, there will be cases where we will need to include some additional attributes.

We have previously mentioned that the type attributes allow us to choose between two different types of input settings, basic and specialized, but what exactly are they?

## Basic input types

The basic input type is a set of options that allow us to include various types of input settings within our theme editor. Under the basic category, we can use the following options:

- `checkbox`
- `number`
- `radio`
- `range`
- `select`
- `text`
- `textarea`

As we have mentioned previously, most of the input settings will contain only the standard attributes. However, some of the specialized inputs and even basic inputs will require additional attributes. Let's now look into each input type, learn how to use it, and what type of results we can expect.

## The checkbox input

The checkbox type of input, as its name suggests, is a Boolean type of field that allows us to create a checkbox option within the theme editor:

```json
{
  "type": "checkbox",
  "id": "enable_popup",
  "label": "Enable popup",
  "default": true
}
```

As we can see, the checkbox input contains three mandatory and one optional attribute whose value we have set to true. Otherwise, if we were to remove the `default` attribute, the default state of the checkbox will be `false`. The following screenshot shows us an example of the checkbox input type:

![](img/4e10d2f6d65d6d1c551f760d91c2c116_201_0.png)

Figure 6.4 – Example of the checkbox basic input type

We can use the Boolean input type to toggle features on and off, which we can achieve by taking the checkbox input value and checking its current state using an `if` statement:

```liquid
{% if settings.enable_popup == true %}
{% endif %}
```

By checking whether the checkbox input value is equal to true, we have created a simple feature that will enable or disable a certain feature from the store easily, as the code block inside the statement will only render if the statement is true:

```liquid
{% if settings.enable_popup != blank %}
{% endif %}
```
