
At this point in the chapter, WPF might look like nothing more than yet another GUI framework that is providing (more or less) the same services as Windows Forms, MFC, or VB6. If this were in fact the case, you might question the need for yet another UI toolkit. To truly see what makes WPF so unique, you require an understanding of the XML-based grammar, XAML.

## Summary

Windows Presentation Foundation (WPF) is a user interface toolkit introduced with the release of .NET 3.0. The major goal of WPF is to integrate and unify previously unrelated desktop technologies (2D graphics, 3D graphics, window and control development, etc.) into a single, unified programming model. Beyond this point, WPF programs typically make use of XAML, which allows you to declare the look and feel of your WPF elements via markup.

Recall that XAML allows you to describe trees of .NET objects using a declarative syntax. During this chapter’s investigation of XAML, you were exposed to several new bits of syntax, including property-element syntax and attached properties, as well as the role of type converters and XAML markup extensions.

XAML is a key aspect for any production-level WPF application. The final example of this chapter gave you a chance to build a WPF application that showed many of the concepts discussed in this chapter. The next chapters will dive deeper into these concepts as well as introduce many more.

## CHAPTER 25

## WPF Controls, Layouts, Events, and Data Binding

Chapter 24 provided a foundation for the WPF programming model, including an examination of the Window and Application classes, the grammar of XAML, and the use of code files. Chapter 24 also introduced you to the process of building WPF applications using the designers of Visual Studio. In this chapter, you will dig into the construction of more sophisticated graphical user interfaces using several new controls and layout managers, learning about additional features of the WPF Visual Designer for XAML of Visual Studio along the way.

This chapter will also examine some important related WPF control topics such as the data-binding programming model and the use of control commands. You will also learn how to use the Ink and Documents APIs, which allow you to capture stylus (or mouse) input and build rich text documents using the XML Paper Specification, respectively.

## A Survey of the Core WPF Controls

Unless you are new to the concept of building graphical user interfaces (which is fine), the general purpose of the major WPF controls should not raise too many issues. Regardless of which GUI toolkit you might have used in the past (e.g., VB6, MFC, Java AWT/Swing, Windows Forms, macOS, or GTK+/GTK# [among others]), the core WPF controls listed in Table 25-1 are likely to look familiar.

## Table 25-1. The Core WPF Controls

| WPF Control Category | Example Members | Meaning in Life |
|---|---|---|
| Core user input controls | Button, RadioButton, ComboBox, CheckBox, Calendar, DatePicker, Expander, DataGrid, ListBox, ListView, ToggleButton, TreeView, ContextMenu, ScrollBar, Slider, TabControl, TextBlock, TextBox, RepeatButton, RichTextBox, Label | WPF provides an entire family of controls you can use to build the crux of a user interface. |
| Window and control adornments | Menu, ToolBar, StatusBar, ToolTip, ProgressBar | You use these UI elements to decorate the frame of a Window object with input devices (such as the Menu) and user informational elements (e.g., StatusBar and ToolTip). |
| Media controls | Image, MediaElement, SoundPlayerAction | These controls provide support for audio/video playback and image display. |
| Layout controls | Border, Canvas, DockPanel, Grid, GridView, GridSplitter, GroupBox, Panel, TabControl, StackPanel, Viewbox, WrapPanel | WPF provides numerous controls that allow you to group and organize other controls for the purpose of layout management. |

Note The intent of this chapter is not to walk through each and every member of each and every WPF control. Rather, you will receive an overview of the various controls with an emphasis on the underlying programming model and key services common to most WPF controls.

## The WPF Ink Controls

In addition to the common WPF controls listed in Table 25-1, WPF defines additional controls for working with the digital Ink API. This aspect of WPF development is useful during Tablet PC development because it lets you capture input from the stylus. However, this is not to say a standard desktop application cannot leverage the Ink API because the same controls can capture input using the mouse.

The System.Windows.Ink namespace of PresentationCore.dll contains various Ink API support types (e.g., Stroke and StrokeCollection); however, a majority of the Ink API controls (e.g., InkCanvas and InkPresenter) are packaged up with the common WPF controls under the System.Windows.Controls namespace in the PresentationFramework.dll assembly. You'll work with the Ink API later in this chapter.

## The WPF Document Controls

WPF also provides controls for advanced document processing, allowing you to build applications that incorporate Adobe PDF-style functionality. Using the types within the System.Windows.Documents namespace (also in the PresentationFramework.dll assembly), you can create print-ready documents that support zooming, searching, user annotations (sticky notes), and other rich text services.

Under the covers, however, the document controls do not use Adobe PDF APIs; rather, they use the XML Paper Specification (XPS) API. To the end user, there will really appear to be no difference because PDF documents and XPS documents have an almost identical look and feel. In fact, you can find many free utilities that allow you to convert between the two file formats on the fly. Because of space limitation, these controls won’t be covered in this edition.

## WPF Common Dialog Boxes

WPF also provides you with a few common dialog boxes such as OpenFileDialog and SaveFileDialog. These dialog boxes are defined within the Microsoft.Win32 namespace of the PresentationFramework.dll assembly. Working with either of these dialog boxes is a matter of creating an object and invoking the ShowDialog() method, like so:

```
using Microsoft.Win32;
//omitted for brevity
private void btnShowDlg_Click(object sender, RoutedEventArgs e)
{
    // Show a file save dialog.
    SaveFileDialog saveDlg = new SaveFileDialog();
    saveDlg.ShowDialog();
}
```

As you would hope, these classes support various members that allow you to establish file filters and directory paths and gain access to user-selected files. You will put these file dialogs to use in later examples; you will also learn how to build custom dialog boxes to gather user input.

## A Brief Review of the Visual Studio WPF Designer

A majority of these standard WPF controls have been packaged up in the System.Windows.Controls namespace of the PresentationFramework.dll assembly. When you build a WPF application using Visual Studio, you will find most of these common controls contained in the toolbox, provided you have a WPF designer open as the active window.

Similar to other UI frameworks created with Visual Studio, you can drag these controls onto the WPF window designer and configure them using the Properties window (which you learned about in Chapter 24). While Visual Studio will generate a good amount of the XAML on your behalf, it is not uncommon to edit the markup yourself manually. Let’s review the basics.

## Working with WPF Controls Using Visual Studio

You might recall from Chapter 24 that when you place a WPF control onto the Visual Studio designer, you want to set the x:Name property through the Properties window (or through XAML directly) because this allows you to access the object in your related C# code file. You might also recall that you can use the Events tab of the Properties window to generate event handlers for a selected control. Thus, you could use Visual Studio to generate the following markup for a simple Button control:

```
<Button x:Name="btnMyButton" Content="Click Me!" Height="23" Width="140" Click="btnMyButton_Click" />
```
