While we cannot include additional attributes, we can use the `content` attribute to create a direct link to a specific page, providing some additional information. We can do this by including the necessary information by using the following format:

```
```text
[link text](link url)
```
```

Including the direct link to a specific page will make our theme editor more concise. At the same time, it also allows us to include all the necessary information that someone using it might require:

```
```json
{
  "type": "paragraph",
  "content": "The paragraph type setting allows us to include a direct link to any page. [Learn more](https://shopify.dev/themes/architecture/settings/sidebar-settings#header)"
}
```
```

Note that we can create a direct link to any page within both the `content` and the `info` attributes on any type of setting that accepts the `info` attribute.

We have now learned about every type of input and sidebar type of setting. However, while it is essential to know all the current settings, it is also essential to mention some of the now deprecated settings, but which we may still encounter in our everyday work.

# Glancing at the deprecated settings

While the following settings are no longer supported, there is a high probability that we might encounter them on older themes that their store owner never updated. Since these settings are no longer supported, we will not go into that much detail on how they work, but we will provide some general guidelines for identifying them and what they do.

# The font input

The font input type of setting allowed us to generate a shortlist of font files accessible on Shopify:

```
```json
{
  "type": "font",
  "id": "body_font",
  "label": "Body font"
}
```
```