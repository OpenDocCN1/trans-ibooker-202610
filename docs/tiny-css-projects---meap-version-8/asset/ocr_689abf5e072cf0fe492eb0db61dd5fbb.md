# Hotels

# Meeru Island Resort & Spa

The stylish Meeru Island Resort & Spa strikes the perfect note for a peaceful escape.

Learn more

# Flamingo Beach

pools and activities to keep children smiling, plus comfy apartments that are tailored for families.

Learn more

# Protur Safari Park

The big, family-friendly complex dishes up plenty of pools and activities.

Learn more

# Mountain View Resort

There are heaps of spots to soak in at the super-sized Holiday Village Costa del Sol.

Learn more

Figure 5.7 Grid applied to <main>

# 5.2.1 Media queries

At the moment our cards are vertically stacked. This is the default behavior in most cases with HTML elements and makes sense on mobile devices where device width is rather narrow. However, for desktop screens, where the browser window can be much wider, we can take advantage of the horizontal space by using media queries. We can define some media queries to adjust the layout.

+   1. If the window width is greater than or equal to 700px, we adjust the grid to have 2 equally sized columns, and set the height of each section to exactly 350pixels.

+   2. At 950 pixels, we adjust the layout again to have 4 equally sized columns overriding the grid-template-columns value set in the previous media query. The height property value still remains 350 pixels since the condition for the previous media query (min-width: 700px) is still being met.

```
min-width: 700px
```

If neither of the requirements for these media queries are met (when the browser window is less than 700px wide) then the cards will be stacked vertically in a single column. Listing 5.6 shows the 2 media queries being created.