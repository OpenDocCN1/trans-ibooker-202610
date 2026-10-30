Using a standard drag-and-drop operation, you can place any of these controls onto the window’s designer surface or drag the control into the XAML markup editor at the bottom of the designer. When you do, the initial XAML will be authored on your behalf. Use your mouse to drag a Button control and a Calendar control onto the designer surface. After you have done so, notice how you can relocate and resize your controls (and be sure to examine the resulting XAML generated based on your edits).

In addition to building the UI via the mouse and toolbox, you can manually enter your markup using the integrated XAML editor. As you can see in Figure 24-5, you do get IntelliSense support, which can help simplify the authoring of the markup. For example, try to add the Background property to the opening <Window> element.

![](bbox=[0.105, 0.26, 0.925, 0.585])

Figure 24-5. The WPF Window designer

Take a few moments to add some property values directly in the XAML editor. Be sure you take the time to become comfortable using this aspect of the WPF designer.

# Setting Properties Using the Properties Window

After you have placed some controls onto your designer (or manually defined them in the editor), you can then make use of the Properties window to set property values for the selected control, as well as rig up event handlers for the selected control. By way of a simple test, select your Button control on the designer. Now, use the Properties window to change the Background color of the Button using the integrated Brushes editor (see Figure 24-6; you will learn more about the Brushes editor in Chapter 26, during your examination of WPF graphics).