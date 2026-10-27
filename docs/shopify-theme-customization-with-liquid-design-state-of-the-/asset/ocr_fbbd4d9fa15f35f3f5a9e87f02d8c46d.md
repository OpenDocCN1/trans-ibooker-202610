# The name attribute

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