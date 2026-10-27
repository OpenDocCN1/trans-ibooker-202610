
Note that including the same section file across multiple templates will result in the same content being visible across all templates. By configuring the static section from the theme editor, we can save the selected data inside the `settings_data.json` file, which will return the exact data for any occurrence of the same section.

If we need to repeat the static section multiple times with different content, we would need to create a new section file using a different name:

```
```liquid
{% section "related-product-1" %}
{% section "related-product-2" %}
```
```

So far, we have seen what static sections are and how to use them to create a template-based configurable layout. But we also have access to dynamic sections, which we do not have to include every time we want to reposition a section manually.

As the name suggests, **dynamic sections** are a set of sections that we can add, remove, reposition, or repeat any number of times with different content, without touching a single line of code, all from the theme editor. Let’s return to the home page within the theme editor and look at some of the existing dynamic sections:

![](img/4e10d2f6d65d6d1c551f760d91c2c116_243_0.png)

*Figure 7.2 – Example of page sections within the theme editor*

We can easily distinguish the static section from the dynamic section by simply hovering over the section itself. We will notice two icons to the right of the dynamic section's name – an *eye* icon and *six dots* – by hovering over it. The eye icon will allow us to toggle the section's visibility on and off. By clicking and holding onto the six dots icon, we can reposition the section by moving it above or below the other sections.

Additionally, at the bottom of the dynamic sections, we will notice an **Add section** button, which, once clicked, will show a dropdown that we can use to include any number of sections that exist in our store by simply clicking on them. Any section that's added to the theme via the **Add section** button will allow us to include different content for any occurrence, and we can repeat this as many times as we want.

Until recently, the home page was the only place where we could use dynamic sections. For all other templates, we had to rely on using static sections. However, since the Shopify Unite 2021 event, Shopify has introduced JSON type templates, which we briefly mentioned in *Chapter 1, Getting Started with Shopify*. JSON templates, which we will learn more about later in this chapter, add dynamic sections to any page and bring the store's entire functionality to a whole new level.

Now that we have learned about what sections are and how we can use them, it is time to learn how to create them.

## Working with the section schema

In the previous chapter, we learned how to use JSON to create global settings, which has a similar format to the JSON for section files. However, sectional JSON comes with a few distinct differences.

The first major difference is that we need to define JSON inside the section file instead of the `settings_schema.json` file. To do this, we will need to introduce the `schema` tag:

```
{% schema %}{% endschema %}
```

The `schema` tag is a Liquid tag that does not have any output on its own. It simply allows us to write JSON code inside the section files. Note that each section file can only contain a single `schema` tag, which must stand on its own. It cannot be nested inside any kind of other Liquid tags.

Once we have the `schema` tag in place, we are ready to familiarize ourselves with the types of attributes that we can use within the `schema` tags.

## The name attribute

As its name suggests, the name attribute allows us to set the section name, which we will use to identify the section inside the theme editor later:

```
{% schema %}
{
  "name": "Announcement bar"
}
{% endschema %}
```

With that, we have learned how to define the name of the section file, but what if we wanted to create an international store where we can easily translate not only the store content, but also the store setting labels, inside the theme editor?

We can easily translate most of the attributes inside the schema tag by including the translation keys in the name attribute value:

```
{% schema %}
{
  "name": {
    "cs": "Panel oznámení",
    "da": "Meddelel seslinje",
    "de": "Ankündigungsbereich",
    "en": "Announcement bar"
  }
}
{% endschema %}
```

The first value represents the name of the file inside the Locales directory, whereas the second one represents the translated value. By introducing the translating keys, we have ensured that as soon as we change our store language, the name value will automatically adjust and present the value of the currently selected language file.

Note that we can include the translation keys for different kinds of attributes, including some of the input settings that we learned about in the previous chapter. The attributes that we can use the translation keys with include name, info, label, group, placeholder, unit, content, and category.

Note that name is a mandatory attribute. However, the name attribute does not have to be unique compared to the other sections, so we should pay attention when creating new sections to avoid confusion.

## The class attribute

class is a simple attribute that allows us to add additional classes to the div element, which wraps around the section content:

```
{% schema %}
{
  "name": {
    "cs": "Panel oznámení",
    "da": "Meddelelseslinje",
    "de": "Ankündigungsbereich",
    "en": "Announcement bar"
  },
  "class": "homepage-section desktop-only"
}
{% endschema %}
```

Through this optional attribute, we can easily include any number of classes in the parent element wrapping around our section, as shown in the following code block:

```
<div id="shopify-section-[id]" class="homepage-section desktop-only">
</div>
```

Note that while we can include any number of classes, we cannot dynamically modify them since the class attribute only accepts string values.

## The settings attribute

Using the settings attribute, we can create section-specific settings, which allows us to configure the section using the theme editor:

```
{% schema %}
{
  "name": {
    "cs": "Panel oznámení",
    "da": "Meddelelseslinje",
    "de": "Ankündigungsbereich",
    "en": "Announcement bar"
  },
```

```
```text
"class": "homepage-section desktop-only",
"settings": [
]
}
{% endschema %}
```
```

Once we have defined the settings attribute, we can start including any input settings, whether they're basic or specialized, that we have previously learned about to create the flow that we need. Since we are working on creating an announcement bar, we can include the following inputs:

```
```liquid
{% schema %}
{
  "name": {
    "cs": "Panel oznámení",
    "da": "Meddelelseslinje",
    "de": "Ankündigungsbereich",
    "en": "Announcement bar"
  },
  "class": "homepage-section desktop-only",
  "settings": [
    {
      "type": "text",
      "id": "announcement-text",
      "label": "Text"
    },
    {
      "type": "color",
      "id": "announcement-text-color",
      "label": "Text color",
      "default": "#000000"
    }
  ]
}
{% endschema %}
```
```

With the introduction of settings and input settings, we have successfully created the first section, whose text and text color values can be adjusted within the theme editor. If we were to try and include this section as a static section, the two input settings that we created previously would immediately be visible under the respective section:

```
{% section "section-file-name" %}
```

However, if we were to navigate to the home page and include the announcement section via the Add section button, we would not find it. We are still missing one attribute, which we need to create a section that we can dynamically add to the home page.

## The presets attribute

The presets attribute allows us to define the default configuration of a section, which makes the section accessible from the Add section dropdown. The presets attribute can contain the following attributes:

- The name attribute is mandatory and will define how the section will appear under the Add section dropdown.
- The category attribute is not mandatory. We can use it to group different sections under a single category for more straightforward navigation.

Note that it is advisable to use a unique name for the presets section to avoid confusion, even though Shopify does not strictly require it. Otherwise, we might end up with multiple sections with similar names:

```
{% schema %}
{
  "name": {
    "cs": "Panel oznámení",
    "da": "Meddelel seslinje",
    "de": "Ankündigungsbereich",
    "en": "Announcement bar"
  },
  "class": "homepage-section desktop-only",
  "settings": [
    {
      "type": "text",
      "id": "announcement-text",
      "label": "Text"
```
