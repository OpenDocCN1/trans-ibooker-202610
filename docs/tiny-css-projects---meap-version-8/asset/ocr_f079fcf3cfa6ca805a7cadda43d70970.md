# Title of our article (heading 1)

Posted on May 16 by Lisa.

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque tincidunt dapibus eleifend. Nam eu urna ipsum. Etiam consequat ac dolor et dapibus. Duis eros arcu, interdum eu volutpat ac, lacinia a tortor. Vivamus justo tortor, porttitor in arcu nec, pretium viverra ipsum. Nam sit amet nibh magna. Sed ut imperdiet orci, id finibus justo. Maecenas magna mauris, tempor nec tempor id, aliquam et nibh. Nunc elementum ut purus id eleifend. Phasellus pulvinar dui orci, sed eleifend magna ullamcorper sit amet. Proin iaculis lacus congue aliquam sodales.

+   1. List item 1
+      - Nested item 1
+      - Nested item 2
+   2. List item 2
+   3. List item 3
+   4. List item 4

![](bbox=[0.18, 0.33, 0.32, 0.4])

Curabitur id augue nulla. Aliquam purus urna, aliquam eu ornare id, maximus et tellus. Aliquam eleifend sem vitae urna blandit, non bibendum tellus dignissim. Aliquam imperdiet imperdiet sapien sit amet consectetur. Nam convallis turpis felis, sed vulputate lacus eleifend a. Mauris pharetra imperdiet lacinia. Sed sit amet feugiat lectus, in consectetur magna. Vestibulum accumsan porta enim at ultricies. Vestibulum vitae massa quis massa dignissim imperdiet.

> Nunc eleifend nulla lobortis porta rhoncus. Vivamus feugiat, sem vitae feugiat aliquam, orci nulla venenatis libero, vitae rhoncus nibh neque ac velit.

Etiam tempor vulputate varius. Duis at metus ut eros ultrices facilisis. Donec ut est finibus, egestas nisl eu, placerat neque. Pellentesque cursus, turpis nec sollicitudin sodales, nisi tellus ultrices lectus, nec facilisis purus neque vitae diam. Nunc eleifend nulla lobortis porta rhoncus. Vivamus feugiat, sem vitae feugiat

# Figure 1.23 Using the universal selector to change the font type on all elements

# NAMESPACE AND THE UNIVERSAL SELECTOR

It is possible to mix different languages together in the same document, such as embedding SVG code into HTML.

The issue with doing that is that it’s difficult to distinguish between the various types of code. For example, we could have a link in our SVG graphic and a link in our HTML but we may want to style the SVG links differently than our HTML links.

To solve this we can use namespaces to uniquely identify code and set our definition of what that code does. Then, in the CSS, we can select that namespace and create styles just for that type of code.

In HTML5 when we insert code into a HTML file that is a known foreign element such as an SVG, MathML or XML it is automatically given its correct namespace.

Here a few examples of coding languages and their namespaces:

+   - HTML elements use the namespace of http://www.w3.org/1999/xhtml
+   - SVG elements use the namespace of http://www.w3.org/2000/svg
+   - MathML elements use the namespace of http://www.w3.org/1998/Math/MathML