![](bbox=[0.14, 0.09, 0.86, 0.21])

Figure 5-19. Three bar charts showing sales by product, category, and country

This works, but it takes up space and isn’t always necessary. Often, different users want to use different views—for example, the product manager may want to use the product view, the category manager may want the category view, and leadership may want the country view. But they don’t all need to see everything at once, and field parameters help us resolve this issue. Instead of showing all three visuals, we let the user choose what they want to see. This keeps the report clean while still offering flexibility.

Let’s walk through how to set up two types of field parameters: one that switches dimensions and one that switches measures.

# Creating field parameters

We need to create a field parameter before we can use it in our report. In the report view in Power BI, we go to Modeling, click on the “New parameter” icon, and choose Fields (see Figure 5-20).