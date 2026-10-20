![](bbox=[0.216, 0.092, 0.784, 0.405])

Figure 5-30. Switching between Format and Measure views by clicking on the listbox

Let’s assume we have a table with countries, currencies, and the format we want to use. Each country can have a different currency and format (see Figure 5-31).

![](bbox=[0.185, 0.549, 0.817, 0.715])

Figure 5-31. Table with countries, currencies, and corresponding format strings

Now, we can go to our Sales measure and change the format to Dynamic. In the format listbox, we set the string to a DAX expression that returns the correct format string based on the selected country. Here’s an example: