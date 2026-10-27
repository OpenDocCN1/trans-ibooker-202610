
The introduction of the `font_picker` input type, which we have mentioned previously, made the font input type of setting obsolete.

### The snippet input

As its name suggests, the `snippet` input type allowed us to select any snippet file that we created previously within our theme and execute its content in a specific position:

```json
{
  "type": "snippet",
  "id": "featured_products",
  "label": "Featured products"
}
```

The introduction of `sections`, which we will learn about in the following chapter, has made the `snippet` input type of setting obsolete.

### Summary

In the previous chapters, we have abstained from going over every specific option, consequently creating a list of options that we need to learn to keep everything concise and to the point. However, in this chapter, we have gone over each option and carefully explained how to use it, when, and what we can expect, since, on account of their importance, we will use the knowledge that we have learned in this chapter on a regular basis.

We have learned how to generate some of the most basic input types, which we can use to output various types of content and types of input, allowing us to create additional logic related to specific content. Additionally, we have also learned how to output specialized input types, allowing us to create complex features with an easy and configurable interface.

Lastly, we have learned how we can organize the JSON settings into separate blocks for better readability. The knowledge that we have attained through this chapter will be helpful and tested in the following chapter.

In the next chapter, we will learn about sections and blocks, and use them to create section/block-specific settings that merchants can use through the theme editor to create re-usable sections.

### Questions

1. What are the two types of input settings?

2. What’s the issue that will cause an error with the following piece of code?

```json
{
  "type": "text",
  "id": "header_announcement",
  "label": "Text",
}
```

3. How can we include a custom font file within Shopify and use it throughout the theme editor?

4. What are the two issues that will prevent us from executing the following piece of code?

```json
{
  "type": "range",
  "id": "number_of_products",
  "min": 110,
  "max": 220,
  "step": 1,
  "unit": "pro",
  "label": "Number of products",
  "default": 235
}
```

## Working with Static and Dynamic Sections

In the previous chapter, we familiarized ourselves with the different types of inputs, whether they're basic or specialized, and how we can use them to create global settings that we can easily configure through the theme editor.

In this chapter, we will not only get to use the previously mentioned inputs, but we will also learn how to create easily configurable and reusable sections that we can use to change the page or even templates' layouts easily.

We will learn about the following topics in this chapter:

- Static versus dynamic sections
- Working with the section schema
- Building with blocks
- Enhancing pages with JSON templates
- Exploring section-specific tags

By the time we complete this chapter, we will understand what sections are, when to use them, and how to create one. We will also learn about the difference between static and dynamic sections, and how we can configure them through the theme editor.

By learning about the section schema and the different attributes that we can use, we will also learn how to create reusable block modules within a section that we can use to repeat and receive different results. After familiarizing ourselves with sections and blocks, we will learn how to improve these concepts by learning about JSON templates and built-in metafields. Lastly, we will learn about the different types of specialized tags that we can use within section files, which will help us create reusable and dynamic modules.

### Technical requirements

While we will explain each topic and have it presented with the accompanying graphics, we will need an internet connection to follow the steps outlined in this chapter, considering that Shopify is a hosted service.

The code for this chapter is available on GitHub: https://github.com/PacktPublishing/Shopify-Theme-Customization-with-Liquid/tree/main/Chapter07.

The Code in Action video for the chapter can be found here: https://bit.ly/3hQzhVg

### Static versus dynamic sections

In *Chapter 1, Getting Started with Shopify*, while discussing the theme structure, we briefly mentioned section files, but *what exactly are sections?*

Besides being the name of one of the directories in the theme file, a **section** is a type of file that allows us to create reusable modules that we can customize using the theme editor, as we learned previously. However, as opposed to the global settings that we have learned about, the major difference is that the JSON settings for sections are defined inside each section file and are section-specific.

Section-specific settings allow us to reuse the same section module multiple times on the page and select a different set of options for each occurrence, making it a pretty powerful feature. For example, we can create a featured collections section and repeat it three times to display three products from three separate collections.

Before we proceed, let's navigate to the theme editor by clicking the **Customize** button from the code editor and see the section files in action. Clicking the **Customize** button will automatically open the theme editor and position us on the home page, with the page preview on the right-hand side and a sidebar on the left. Within the sidebar, we can see several sections that are available on this specific page:

![](img/4e10d2f6d65d6d1c551f760d91c2c116_241_0.png)

Figure 7.1 – Example of page sections within the theme editor

At first glance, we can see that the Header and Footer sections are separated from the rest of the sections by a thin border. This tells us that the Header and Footer sections are static sections whose positions we cannot change from within the theme editor.

Similarly, as with snippet files, static sections are sections that we can manually insert within the theme files, which we can do using the section theme tag:

```liquid
{% section "name-of-the-file" %}
```

By including the section tag, we will automatically render the content of the section to the position where we have included the tag and be able to customize it further using its JSON settings. Note that since this is a static section that we have manually included within the theme files, any settings that we apply to this specific section through the theme editor will be visible across all the pages where we have included this section.

For example, the theme developers have included the header and footer section files inside the `theme.liquid` layout file, which is our theme master file, meaning that the header and footer sections will be visible on any page on our store. On the other hand, if we were to navigate to a different page within our theme editor, we would see a header, a footer, and an entirely different set of sections between those two than those we previously saw on the home page.

By including the header and footer sections inside the `theme.liquid` file, we have made them visible across the entire theme on any page. However, we will not include all the sections within the `theme.liquid` file as they are not needed. Instead, we will include certain template-specific sections inside their respective template files so that they can only be accessed when visiting pages with the specific template assigned.

Let's say that we wanted to include the related products section on the product pages. Here, we would navigate to the `product.liquid` template and include the `section` tag at any position, which would make the related products section visible on any product that uses this particular template.

As we recall from *Chapter 4, Diving into Liquid Core with Objects*, in *The `content_for_layout` object section*, we mentioned that the `content_for_layout` layout object allows us to connect the other templates to the `theme.liquid` file by loading the dynamically generated content, including the section files from other templates, into the `theme.liquid` file.

Since `content_for_layout` outputs the content of all other templates, by placing it between the header and footer sections, we have ensured that we will position all the section files from other template files in between the header and footer sections. Let's look at the `theme.liquid` layout file; we will notice that we positioned `content_for_layout` between the header and footer sections, as depicted in the following block of code:

```liquid
{% section 'header' %}
<div class="page-container drawer-page-content"
     id="PageContainer">
  <main class="main-content js-focus-hidden"
        id="MainContent" role="main" tabindex="-1">
    {{ content_for_layout }}
  </main>
  {% section 'footer' %}
```
