Obviously, a window that can contain only a single control is of little use. When a window needs to contain multiple elements, those elements must be arranged within any number of panels. The panel will contain all of the UI elements that represent the window, after which the panel itself is used as the single object assigned to the Content property.

The System.Windows.Controls namespace provides numerous panels, each of which controls how subelements are maintained. You can use panels to establish how the controls behave if the end user resizes the window, if the controls remain exactly where they were placed at design time, if the controls reflow horizontally from left to right or vertically from top to bottom, and so forth.

You can also intermix panel controls within other panels (e.g., a DockPanel that contains a StackPanel of other items) to provide a great deal of flexibility and control. Table 25-2 documents the role of some commonly used WPF panel controls.

# Table 25-2. Core WPF Panel Controls

| Panel Control | Meaning in Life |
|---|---|
| Canvas | Provides a classic mode of content placement. Items stay exactly where you put them at design time. |
| DockPanel | Locks content to a specified side of the panel (Top, Bottom, Left, or Right). |
| Grid | Arranges content within a series of cells, maintained within a tabular grid. |
| StackPanel | Stacks content in a vertical or horizontal manner, as dictated by the Orientation property. |
| WrapPanel | Positions content from left to right, breaking the content to the next line at the edge of the containing box. Subsequent ordering happens sequentially from top to bottom or from right to left, depending on the value of the Orientation property. |

In the next few sections, you will learn how to use these commonly used panel types by copying some predefined XAML data into the kaxaml.exe application you installed in Chapter 24. You can find all these loose XAML files contained inside the PanelMarkup subfolder of your Chapter 25 code download folder. When working with Kaxaml, to simulate resizing a window, change the height or width of the Page element in the markup.

# Positioning Content Within Canvas Panels

If you come from a WinForms background, you will probably feel most at home with the Canvas panel because it allows for absolute positioning of UI content. If the end user resizes the window to an area that is smaller than the layout maintained by the Canvas panel, the internal content will not be visible until the container is stretched to a size equal to or larger than the Canvas area.

To add content to a Canvas, you begin by defining the required controls within the scope of the opening and closing Canvas tags. Next, specify the upper-left corner for each control; this is where the rendering should begin using the Canvas.Top and Canvas.Left properties. You can specify the bottom-right area indirectly in each control by setting its Height and Width properties or directly by using the Canvas.Right and Canvas.Bottom properties.

To see Canvas in action, open the provided SimpleCanvas.xaml file using kaxaml.exe. You should see the following Canvas definition (if loading these examples into a WPF application, you will want to change the Page tag to a Window tag):