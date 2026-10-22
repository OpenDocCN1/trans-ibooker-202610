# 5.2 Laying out the page using grid

A good starting point is to look at the layout of our cards and the web page as a whole. We need to consider 3 aspects of our layout:

+   - The header and main content
+   - The container for the cards
+   - The content within the cards

We will use the grid layout module for all 3 use cases.

> **Grid**
> Grid allows us to place and align elements across both the vertical and horizontal axis on a system of columns and rows.
> 
> Check out chapter 2 if you need a refresher on how grid works.

To lay out the elements on our page we will start with creating the styles for narrow screens and then edit the layout as we build up to larger screen sizes via media queries.

Our layout consists of 2 landmarks, <header> and <main>, which are immediate children of <body> (Listing 5.3). By giving the body a display property a value of grid we will be affecting the position of the header and main elements.

```
Listing 5.3 Starting HTML
<body>
  <header>  #A
  ...       #A
  </header> #A
  <main>    #B
  ...       #B
  </main>   #B
</body>
```

#A Contains the page header
#B Contains the cards

We then use the place-item property to center the elements on the page. This property is the short-hand for combining declaring values for the align-items and justify-items properties. We will set its value to center, aligning all the items in the middle of their respective rows and columns. Our updated body rule is shown in Listing 5.4.