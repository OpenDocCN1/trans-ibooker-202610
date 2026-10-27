Note that while we can include HTML code inside the html type input, we cannot include Liquid code inside the field as it will get processed as a simple string.

# The linklist input

Using the link_list input type, we can create a special menu picker type of field that allows us to output one of the store navigation menus. Note that we can only see the menus we have previously created inside the admin Navigation section, located under the Online store section:

```
{
  "type": "link_list",
  "id": "header-menu",
  "label": "Header Menu",
  "default": "main-menu"
}
```

Note that while the default attribute is optional, it only accepts two specific values, main-menu and footer. Let's see how the linklist input looks in the following screenshot:

![](bbox=[0.305, 0.517, 0.662, 0.862])

Figure 6.13 – Example of the linklist specialized input type