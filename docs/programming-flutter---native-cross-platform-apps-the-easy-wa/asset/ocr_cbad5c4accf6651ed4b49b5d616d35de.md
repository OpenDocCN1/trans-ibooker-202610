width and height are incompatible with the image’s and repetition is not allowed.

+   - headers sets the headers for the HTTP request using a Map<String, String>.
+   - A semanticLabel String for screen reading accessibility services and an excludeFromSemantics bool if it’s explicitly not needed.

# Use Width, Height, and Repetition Attributes to Display Images the Way You Want Them

To further clarify how width, height, and repeat are used, we’ll look at a few screenshots showing different combinations of size and repetition settings.

If you don’t set anything at all, and just give a URL, like this:

```
Image.network(
  url
)
```

the image will be fitted to the view, as you can see in the first screenshot.

![](bbox=[0.15, 0.505, 0.33, 0.76])

If you set a width lower than the view’s width, like this:

```
Image.network(
  url,
  width: 200.0
)
```