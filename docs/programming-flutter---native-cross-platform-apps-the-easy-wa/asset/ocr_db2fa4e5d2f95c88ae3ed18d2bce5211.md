![](bbox=[0.145, 0.09, 0.616, 0.531])

Representing Flutter apps as trees is a very effective way to visualize the layout of a Flutter app.

In this case, the app’s main structure will be a `Column` with three children:

+   - The calculator’s screen, which will a `Card` with some `Text` on it.
+   - A `Row` containing the button that deletes the entire expression and the button that deletes one character at a time, which will be a couple of `FlatButtons`.
+   - A `Row` for the rest of the calculator’s buttons, which contains two `Column` widgets: one for the three columns containing the numbers (which is made up of four `Rows` of four buttons each), and one for the column containing the operator buttons.

To make the layout look and work better, we will be making expansive (no pun intended) use of the `Expanded`, but that will come later on in _Make the App Look and Work Right with Expanded_.