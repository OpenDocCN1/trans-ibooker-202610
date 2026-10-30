

<GridSplitter Grid.Column="0" Width="5" Background="Gray" />
<StackPanel Grid.Column="0" VerticalAlignment="Stretch">
    <Label Name="lblSpellingInstructions" FontSize="14" Margin="10,10,0,0">
        拼写提示
    </Label>

    <Expander Name="expanderSpelling" Header="试试这些！"
        Margin="10,10,10,10">
        <!-- This will be filled programmatically -->
        <Label Name="lblSpellingHints" FontSize="12" />
    </Expander>
</StackPanel>

<!-- This will be the area to type within -->
<TextBox Grid.Column="1"
    SpellCheck.IsEnabled="True"
    AcceptsReturn="True"
    Name="txtData" FontSize="14"
    BorderBrush="Blue"
    VerticalScrollBarVisibility="Auto"
    HorizontalScrollBarVisibility="Auto">
</TextBox>
</Grid>
```

## 实现 MouseEnter/MouseLeave 事件处理程序

此时，窗口的 UI 已经完成。剩下的唯一任务是为其余的事件处理程序提供实现。首先更新 C# 代码文件，使 MouseEnter、MouseLeave 和 MouseExit 处理程序都能在状态栏的文本区域显示恰当的消息，以帮助最终用户，如下所示：

```
public partial class MainWindow : System.Windows.Window
{
    ...
    protected void MouseEnterExitArea(object sender, RoutedEventArgs args)
    {
        statBarText.Text = "Exit the Application";
    }
    protected void MouseEnterToolsHintsArea(object sender, RoutedEventArgs args)
    {
        statBarText.Text = "Show Spelling Suggestions";
    }
    protected void MouseLeaveArea(object sender, RoutedEventArgs args)
    {
        statBarText.Text = "Ready";
    }
}
```

此时，你可以运行应用程序。当鼠标悬停在菜单项或工具栏按钮上时，你应该能看到状态栏文字随之变化。

## 实现拼写检查逻辑

WPF API 内置了拼写检查器支持，且不依赖于 Microsoft Office 产品。这意味着你无需通过 COM 互操作层使用 Microsoft Word 的拼写检查器；只需几行代码，就能轻松添加同类支持。

你可能还记得，在定义 TextBox 控件时，你将 SpellCheck.IsEnabled 属性设为了 true。启用此属性后，拼写错误的单词会像在 Microsoft Office 中一样显示红色波浪线。更棒的是，底层编程模型允许你访问拼写检查引擎，从而获取拼写错误单词的建议列表。将以下代码添加到 ToolsSpellingHints_Click() 方法中：

```
protected void ToolsSpellingHints_Click(object sender, RoutedEventArgs args)
{
    string spellingHints = string.Empty;

    // Try to get a spelling error at the current caret location.
    SpellingError error = txtData.GetSpellingError(txtData.CaretIndex);
    if (error != null)
    {
        // Build a string of spelling suggestions.
        foreach (string s in error.Suggestions)
        {
            spellingHints += $"{s}\n";
        }

        // Show suggestions and expand the expander.
        lblSpellingHints.Content = spellingHints;
        expanderSpelling.IsExpanded = true;
    }
}
```

前面的代码很简单。你只需使用 CaretIndex 属性获取文本框中插入符的当前位置，并据此提取 SpellingError 对象。如果该位置存在错误（也就是说，该值不为 null），就可以使用名称恰如其分的 Suggestions 属性遍历建议列表。获取拼写错误单词的所有建议后，将这些数据赋给 Expander 中的 Label。

至此，你已经初步实现了一个可运行的文字处理程序，只用了几行过程式代码（以及大量 XAML）。了解控件命令还能让你的应用程序更上一层楼。

## 理解 WPF 命令

WPF 通过命令体系结构支持一种可被视为与控件无关的事件。典型的 .NET Core 事件定义在特定基类中，且只能由该类或其派生类使用。因此，普通的 .NET Core 事件与其定义所在的类紧密耦合。

相比之下，WPF 命令是类似事件的实体，不依赖于特定控件，而且在很多情况下，可以成功应用于多种（看似毫不相关的）控件类型。例如，WPF 支持复制、粘贴和剪切命令，这些命令可以应用于各种 UI 元素（如菜单项、工具栏按钮和自定义按钮），也可以用于键盘快捷键（如 Ctrl+C 和 Ctrl+V）。

其他 UI 工具包（如 Windows Forms）虽然提供了用于此类用途的标准事件，但使用这些事件通常会产生冗余且难以维护的代码。在 WPF 模型中，你可以改用命令，最终通常可以得到规模更小、灵活性更高的代码库。

## 内置命令对象

WPF 内置了许多控件命令，所有这些命令都可以配置相应的键盘快捷键（或其他输入手势）。从编程角度来看，WPF 命令是任何支持某个属性（通常名为 Command）的对象；该属性返回一个实现了 ICommand 接口的对象，如下所示：

```
public interface ICommand
{
    // Occurs when changes occur that affect whether
    // or not the command should execute.
    event EventHandler CanExecuteChanged;

    // Defines the method that determines whether the command
    // can execute in its current state.
    bool CanExecute(object parameter);

    // Defines the method to be called when the command is invoked.
    void Execute(object parameter);
}
```

WPF 提供了多种命令类，开箱即用地公开了近 100 个命令对象。这些类定义了许多属性，用于公开各个命令对象，而每个对象都实现了 ICommand。表 25-3 列出了一些可用的标准命令对象。

### 表 25-3. 内置的 WPF 控件命令对象

| WPF 类 | 命令对象 | 用途 |
|---|---|---|
| ApplicationCommands | Close、Copy、Cut、Delete、Find、Open、Paste、Save、SaveAs、Redo、Undo | 各种应用程序级命令 |
| ComponentCommands | MoveDown、MoveFocusBack、MoveLeft、MoveRight、ScrollToEnd、ScrollToHome | UI 组件常用的各种命令 |
| MediaCommands | BoostBase、ChannelUp、ChannelDown、FastForward、NextTrack、Play、Rewind、Select、Stop | 各种媒体相关命令 |
| NavigationCommands | BrowseBack、BrowseForward、Favorites、LastPage、NextPage、Zoom | 各种与 WPF 导航模型相关的命令 |
| EditingCommands | AlignCenter、CorrectSpellingError、DecreaseFontSize、EnterLineBreak、EnterParagraphBreak、MoveDownByLine、MoveRightByWord | 各种与 WPF Documents API 相关的命令 |

## 将命令连接到 Command 属性

如果要将任何 WPF 命令属性连接到支持 Command 属性的 UI 元素（如 Button 或 MenuItem），需要做的工作很少。下面通过更新当前菜单系统来演示如何实现：添加一个名为 Edit 的顶级菜单项，以及三个用于复制、粘贴和剪切文本数据的子项，如下所示：

```
<Menu DockPanel.Dock="Top" HorizontalAlignment="Left" Background="White" BorderBrush="Black">
    <MenuItem Header="_File" Click="FileExit_Click">
        <MenuItem Header="_Exit" MouseEnter="MouseEnterExitArea" MouseLeave="MouseLeaveArea" Click="FileExit_Click"/>
    </MenuItem>
    <!-- New menu item with commands! -->
    <MenuItem Header="_Edit">
        <MenuItem Command="ApplicationCommands.Copy"/>
        <MenuItem Command="ApplicationCommands.Cut"/>
        <MenuItem Command="ApplicationCommands.Paste"/>
    </MenuItem>
    <MenuItem Header="_Tools">
        <MenuItem Header="_Spelling Hints" MouseEnter="MouseEnterToolsHintsArea" MouseLeave="MouseLeaveArea" Click="ToolsSpellingHints_Click"/>
    </MenuItem>
</Menu>
```

注意，Edit 菜单中的每个子项都为 Command 属性指定了一个值。这样做后，菜单项会自动获得正确的名称和快捷键（例如，剪切操作对应 Ctrl+C）；同时，应用程序无需任何过程式代码就能支持复制、剪切和粘贴！

运行应用程序并选中一些文本后，就可以直接使用新添加的菜单项了。此外，应用程序还能够响应标准的右键单击操作，并向用户显示相同的选项。

## 将命令连接到任意操作

如果要将命令对象连接到任意（特定于应用程序的）事件，就需要使用过程式代码。实现起来并不复杂，但所需逻辑比 XAML 中的示例稍多一些。例如，假设你希望整个窗口都能响应 F1 键，让最终用户按下该键时启动关联的帮助系统。另外，假设主窗口的代码文件定义了一个名为 SetF1CommandBinding() 的新方法，并在调用 InitializeComponent() 后于构造函数中调用该方法。

```
public MainWindow()
{
    InitializeComponent();
    SetF1CommandBinding();
}
```

