+   1. We can retrieve the path to a file within the Assets directory by encapsulating the file's name within quotation marks, followed by a pipeline, followed by the asset_url filter:

```
{{ "theme.css" | asset_url }}
```

+   2. In the previous example, we have used the name of the stylesheet file within our store in combination with asset_url, which will provide us with the string path toward this specific file: https://cdn.shopify.com/s/files/1/0559/0089/7434/t/4/assets/theme.css?v=10188701410004355449

+   3. Now that we have recovered the path toward our location, as previously mentioned, we have two choices. The first option is to use an HTML link tag to link the CSS file with our theme:

```
<link rel="stylesheet" href="{{ "theme.css" | asset_url }}">
```

Our theme will now have full access to the theme.css file and its rules with the link tag in place:

```
<link rel="stylesheet" href="//cdn.shopify.com/s/files/1/0559/0089/7434/t/4/assets/theme.css?v=10188701410004355449">
```

+   4. However, as we have previously mentioned, besides using the HTML link tag, we can also combine URL filters with an HTML filter to generate the necessary HTML attributes:

```
{{ "theme.css" | asset_url | stylesheet_tag }}
```

In the previous example, we used the asset name whose path we are looking to recover, followed by the asset_url filter, which would usually only return the string path. However, pairing it with stylesheet_tag will automatically generate the HTML link tag with all the necessary attributes:

```
<link href="//cdn.shopify.com/s/files/1/0559/0089/7434/t/4/assets/theme.css?v=10188701410004355449" rel="stylesheet" type="text/css" media="all">
```