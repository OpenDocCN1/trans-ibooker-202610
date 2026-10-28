
## Report View: The Insert Tab

Next on the ribbon under Report view, we move to the Insert tab shown in Figure 2-15.

![](img/f5dc9faf226ccd0a92e48aecbde6db96_36_0.png)

Figure 2-15. The Insert tab in the ribbon.

### The Pages Subsection

In the Pages subsection, the New Page button will create a drop-down menu no matter which portion you click on. That menu gives you two options, Blank Page and Duplicate Page. Blank Page creates a new page in your report farthest to the right of all your other report pages. Duplicate Page will make a copy of your current report page exactly as it is currently configured, with all visuals on the canvas, and you’ll find that new copy farthest to the right of all the other pages in the report.

### The Visuals Subsection

The Visuals subsection has two of the exact same buttons that we saw in the Home tab, the New Visual and More Visuals buttons. Their functionality is exactly the same, too. They are duplicated here as a matter of convenience, so if you're looking for other specific items in this subsection of the ribbon, which revolves around putting things onto the canvas, you can do it from here, as well.

### The AI Visuals Subsection

The next subsection in the Insert tab deals with AI Visuals, shown in Figure 2-15. At the time of this writing, there are four of these AI-powered visuals in general availability. If you click on any of them, that puts a blank version of that visual onto the canvas as far to the top and left-hand corner as possible. We’ll discuss these visuals in more detail in Chapter 3, where we’ll provide a complete rundown of all the visuals that are available in Power BI out of the box.

What’s important to note here is that these visuals in particular offer very unique functionality in terms of analytics capability. Microsoft is keen to demonstrate its AI prowess in as many Power Platform products as possible, and Power BI is no exception. I’d expect more of these AI-powered visuals to come to general availability down the road.

### The Power Platform Subsection

The next subsection, Power Platform, deals with items that are technically visuals, but they are fundamentally different from every other visual in the platform. Power Platform visuals offer the ability to interact with and engage the other portions of the Power Platform that we discussed earlier, embedding them into your Power BI report. We’ll discuss these in more detail later, but know they are very powerful and help demonstrate the Power Platform’s holistic value inside an organization.

### The Elements Subsection

The final subsection of the Insert tab, Elements, deals with report elements. These are items that aren’t necessarily interactive like Power BI visuals, but they can help enhance your report or give it extra clarity. Different Elements choices include the Text Box, Buttons, Shapes, and Image controls.

Text Box in Power BI does the exact same thing it does in PowerPoint. It puts an editable text box onto the canvas, along with a basic formatting tool for your text. This tool allows you to change the font, font size, text color, bolding, italicizing, underlining, text alignment, and to add hyperlinks to web pages outside of your report. Other elements on the Text Box can be edited in the Format Text Box pane that appears when a text box is selected. See an example of what that looks like on the canvas in Figure 2-16.

![](img/f5dc9faf226ccd0a92e48aecbde6db96_41_0.png)

Figure 2-16. The Text Box interface lets you apply all kinds of formatting options.

Press the Buttons element to get a list of options that can help with report navigation or provide additional information. There are quite a few, so Figure 2-17 shows the whole list so you can start thinking about how you might use these in your future report.

![](img/f5dc9faf226ccd0a92e48aecbde6db96_42_0.png)

Figure 2-17. Buttons...buttons as far as the eye can see.

It’s important to note that for all these buttons, except Q&A and Bookmark, placing the button on your canvas doesn’t do anything until you provide Power BI the context for how that button should be used. When you put one of these buttons on your canvas, you’ll see a Format Button pane will appear on the right, and it will contain visual formatting options, noting what action that button should do.

The Q&A and Bookmark buttons have default actions already set to the Q&A and Bookmark functions respectively. However, you can still change these actions to a different action if you feel your report could use that button in a different context.

The Shapes button will show you a wide list of shapes that you can put into a report. The shape, when selected, will appear on your canvas already filled with a default color based on your theme. You can change it from there by manipulating the size of the visual on the canvas. It can also be manipulated when selected for formatting your shape, as shapes have their own special pane.

Finally, if you select the Image button, an Explorer window will pop up so you can select an image to bring into your Power BI report. Power BI can add in pictures from the following formats as shown in Figure 2-18. (As an aside, there is a correct way to pronounce GIF... don't be on the wrong side.)

![](img/f5dc9faf226ccd0a92e48aecbde6db96_43_0.png)

Figure 2-18. Seriously though, there's an image format for just about everybody.

Once your image is on your report page, like with every other item available in the Elements subsection of the ribbon, when selected it will have its own special formatting pane appear on the right side. The shape can, like all other Power BI elements, have its size modified in the report by clicking and dragging the box to be the size desired.

## Report View: The Modeling Tab

The third part of the ribbon in the Report view is the Modeling tab. Here you have more data management options from the Report view. There is a lot of magic in Figure 2-19.

![](img/f5dc9faf226ccd0a92e48aecbde6db96_43_1.png)

Figure 2-19. Modeling your data is critical to being successful in Power BI.

### The Relationships Subsection

First we see Manage Relationships. This button opens a window that lets you see all the relationships in your model and add or make edits to existing relationships from here.

By default, Power BI will try to auto detect relationships based on column names. This can be useful for getting an initial set of relationships together for your data model, but it isn't always perhaps the way you want the model set up. It's important to note that this button is greyed out if you have less than two tables in your data model. We will go into more detail on relationships in the next chapter when we discuss the Model view.

### The Calculations Subsection

The next area involves Calculations. The New Measure and Quick Measure buttons function exactly as described earlier about the Home tab.

The other two items are prompts that will bring up the inline DAX editor that we showed earlier so that you can create either DAX Calculated Columns from the New Column button or DAX Calculated Tables from the New table button. Please be aware, there is an inconvenient quirk with the New Column button. If you don't have a table highlighted in the fields pane before selecting this button, Power BI assumes that the calculated column is going to go into the first table that it sees in that pane.

### The Page Refresh Subsection

The Page Refresh subsection has a Change Detection button that is only relevant in DirectQuery scenarios. You can determine whether you want your pages to refresh when there's a detected change in the data or on a fixed refresh interval. You can simulate this function in Power BI Desktop using this feature.

### The What If Subsection

What If simply lets you create what-if parameters. They will be discussed in further detail in Chapter 6.
