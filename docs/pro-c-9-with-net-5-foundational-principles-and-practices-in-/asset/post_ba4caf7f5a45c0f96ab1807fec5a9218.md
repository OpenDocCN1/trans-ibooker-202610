
## TABLE OF CONTENTS

- Understanding the Syntax of WPF XAML .................................................. 977
  - Introducing XAML ........................................................................ 977
  - XAML XML Namespaces and XAML “Keywords” .............................................. 979
  - Controlling Class and Member Variable Visibility ........................................... 982
  - XAML Elements, XAML Attributes, and Type Converters ...................................... 982
  - Understanding XAML Property-Element Syntax ............................................. 983
  - Understanding XAML Attached Properties .................................................. 984
  - Understanding XAML Markup Extensions .................................................... 985
- Building WPF Applications Using Visual Studio ............................................... 987
  - The WPF Project Templates ................................................................... 988
  - The Toolbox and XAML Designer/Editor ..................................................... 988
  - Setting Properties Using the Properties Window ............................................. 989
  - Handling Events Using the Properties Window ................................................ 991
  - Handling Events in the XAML Editor .......................................................... 992
  - The Document Outline Window ............................................................... 993
  - Enable or Disable the XAML Debugger ........................................................ 994
  - Examining the App.xaml File ................................................................. 995
  - Mapping the Window XAML Markup to C# Code ................................................. 996
  - The Role of BAML ............................................................................. 998
  - Solving the Mystery of Main() ............................................................... 998
  - Interacting with Application-Level Data ..................................................... 999
  - Handling the Closing of a Window Object ................................................... 1000
  - Intercepting Mouse Events ................................................................. 1001
  - Intercepting Keyboard Events .............................................................. 1002
- Summary ...................................................................................... 1003

## Chapter 25: WPF Controls, Layouts, Events, and Data Binding ........................ 1005

- A Survey of the Core WPF Controls ....................................................... 1005
  - The WPF Ink Controls ........................................................................ 1006
  - The WPF Document Controls ................................................................. 1006
  - WPF Common Dialog Boxes ..................................................................... 1007

## The Claude 3 Model Family: Introducing Claude 3.5 Sonnet

April 22, 2025 · Anthropic

We're introducing Claude 3.5 Sonnet, the most performant model in the Claude 3 family. It outperforms both Claude 3 Opus and GPT-4o on a broad array of evaluations, while operating at the same speed and cost as Claude 3 Sonnet.

Claude 3.5 Sonnet is now available on the Anthropic API, in the Claude app, and through first-party partners.

Highlights of Claude 3.5 Sonnet:
- State-of-the-art performance across a wide range of tasks
- Transformational results for agentic and coding tasks
- Groundbreaking improvements in vision
- The ability to run at the speed and cost of Claude 3 Sonnet

![](img/587021f4d5ba039be70a11107130377b_34_0.png)

| Model | SWE-bench Verified | MMLU |
| --- | --- | --- |
| Claude 3.5 Sonnet | 50.8% | 88.7% |
| Claude 3 Opus | 38.2% | 86.8% |

> We believe Claude 3.5 Sonnet is the most capable and safest model we have ever built.

This release represents another step toward accessible, reliable AI that can handle complex real-world tasks safely and effectively.

## TABLE OF CONTENTS

- Building the Ink API Tab................................................................................................................ 1039
- Designing the Toolbar.................................................................................................................... 1039
- The RadioButton Control................................................................................................................ 1040
- Add the Save, Load, and Delete Buttons....................................................................................... 1040
- Add the InkCanvas Control............................................................................................................. 1041
- Preview the Window...................................................................................................................... 1041
- Handling Events for the Ink API Tab............................................................................................. 1041
- Add Controls to the Toolbox.......................................................................................................... 1042
- The InkCanvas Control................................................................................................................... 1043
- The ComboBox Control................................................................................................................... 1045
- Saving, Loading, and Clearing InkCanvas Data............................................................................... 1047
- Introducing the WPF Data-Binding Model...................................................................................... 1048
- Building the Data Binding Tab....................................................................................................... 1048
- Establishing Data Bindings........................................................................................................... 1049
- The DataContext Property............................................................................................................. 1049
- Formatting the Bound Data........................................................................................................... 1050
- Data Conversion Using IValueConverter....................................................................................... 1050
- Establishing Data Bindings in Code.............................................................................................. 1052
- Building the DataGrid Tab............................................................................................................. 1053
- Understanding the Role of Dependency Properties......................................................................... 1055
- Examining an Existing Dependency Property................................................................................. 1057
- Important Notes Regarding CLR Property Wrappers....................................................................... 1059
- Building a Custom Dependency Property....................................................................................... 1060
- Adding a Data Validation Routine.................................................................................................. 1063
- Responding to the Property Change............................................................................................... 1063
- Summary........................................................................................................................................ 1064
- Chapter 26: WPF Graphics Rendering Services.............................................................................. 1065
- Understanding WPF's Graphical Rendering Services..................................................................... 1065
- WPF Graphical Rendering Options................................................................................................ 1066
- Rendering Graphical Data Using Shapes........................................................................................ 1067
- Adding Rectangles, Ellipses, and Lines to a Canvas...................................................................... 1068
