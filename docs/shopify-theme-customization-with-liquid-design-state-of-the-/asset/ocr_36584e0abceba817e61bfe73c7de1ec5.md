First, we will learn about generating HTML elements through **HTML and URL filters**. Second, understanding **media filters** will help us with the product media gallery, one of Shopify's latest additions and one of the most sought-after features. Third, by working on the product accordions project, we will learn how to manipulate the string and array types of data through their respective filters. Lastly, working on a product price discount project will provide us with the necessary knowledge of **math and money filters**.

# Technical requirements

While we will explain each topic and present it with accompanying screenshots, you will need an internet connection to follow the steps outlined in this chapter, considering that Shopify is a hosted service.

The code for this chapter is available on GitHub: https://github.com/PacktPublishing/Shopify-Theme-Customization-with-Liquid/tree/main/Chapter05.

The Code in Action video for the chapter can be found here: https://bit.ly/3zmum4j

# Working with HTML and URL filters

In the previous chapter, we had the chance to see a type of URL filter when we worked on outputting the product images, `{{ image | img_url: "400x400" }}`. However, what exactly are URL filters?

**URL filters** are methods that allow us to output a direct path to the assets on Shopify's **Content Delivery Network (CDN)**. We can use URL filters in many ways. However, URL filters alone only provide a string path to the requested asset. Therefore, to find them helpful, we must pair them with HTML tags, such as the `image` tag, inside which we can add the string path to a specific asset as the `href` attribute. Alternatively, we can combine URL filters with **HTML filters** to automatically generate the necessary HTML element with the necessary attributes. Let's see them in action now.

In *Chapter 1, Getting Started with Shopify*, we learned about the `Assets` directory inside our theme files and how it contains all of the internal assets that our theme requires, such as stylesheets, JavaScript files, font files, and images. However, we will first need to load these files within our theme, by following the next steps, since they will not automatically be accessible to us by simply uploading them within the `Assets` directory: