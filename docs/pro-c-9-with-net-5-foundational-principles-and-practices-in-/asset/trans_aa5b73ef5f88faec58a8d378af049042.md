

注意 Excel 命名空间别名。虽然在与 COM 库交互时定义别名并不是必需的，但它为所有导入的 COM 对象提供了简短的限定符。这不仅减少了输入量，还可以解决 COM 对象名称与 .NET Core 类型冲突的问题。

// 为 Excel 对象模型创建别名。
using Excel = Microsoft.Office.Interop.Excel;

接下来，在 Program.cs 的顶层语句中创建一个 Car 记录列表：

```
List<Car> carsInStock = new List<Car>
{
    new Car {Color="Green", Make="VW", PetName="Mary"},
    new Car {Color="Red", Make="Saab", PetName="Mel"},
    new Car {Color="Black", Make="Ford", PetName="Hank"},
    new Car {Color="Yellow", Make="BMW", PetName="Davie"}
};
```

由于你使用 Visual Studio 导入了 COM 库，因此 PIA 已自动配置，以便将使用的元数据嵌入到 .NET Core 应用程序中。因此，所有 COM Variant 数据类型都实现为 dynamic 数据类型。此外，你还可以使用 C# 可选参数和命名参数。考虑以下 ExportToExcel() 的实现：

```
void ExportToExcel(List<Car> carsInStock)
{
    // Load up Excel, then make a new empty workbook.
    Excel.Application excelApp = new Excel.Application();
    excelApp.Workbooks.Add();

    // This example uses a single workSheet.
    Excel._Worksheet workSheet = (Excel._Worksheet)excelApp.ActiveSheet;

    // Establish column headings in cells.
    workSheet.Cells[1, "A"] = "Make";
    workSheet.Cells[1, "B"] = "Color";
    workSheet.Cells[1, "C"] = "Pet Name";

    // Now, map all data in List<Car> to the cells of the spreadsheet.
    int row = 1;
    foreach (Car c in carsInStock)
    {
        row++;
        workSheet.Cells[row, "A"] = c.Make;
        workSheet.Cells[row, "B"] = c.Color;
        workSheet.Cells[row, "C"] = c.PetName;
    }

    // Give our table data a nice look and feel.
    workSheet.Range["A1"].AutoFormat(Excel.XlRangeAutoFormat.xlRangeAutoFormatClassic2);

    // Save the file, quit Excel, and display message to user.
    workSheet.SaveAs($"{Environment.CurrentDirectory}\Inventory.xlsx");
```

```
excelApp.Quit();
Console.WriteLine("The Inventory.xlsx file has been saved to your app folder");
}
```

此方法首先将 Excel 加载到内存中；然而，你在计算机桌面上看不到它。对于此应用程序，你只关心使用内部 Excel 对象模型。但是，如果你确实想显示 Excel 的 UI，请使用以下额外代码行更新你的方法：

```
static void ExportToExcel(List<Car> carsInStock)
{
    // Load up Excel, then make a new empty workbook.
    Excel.Application excelApp = new Excel.Application();

    // Go ahead and make Excel visible on the computer.
    excelApp.Visible = true;
    ...
}
```

在创建空工作表后，你添加了三个列，其名称与 Car 类的属性相同。然后，你用 List<Car> 的数据填充单元格，并将文件保存为（硬编码的）名称 Inventory.xlsx。

此时，如果你运行应用程序，你将能够打开 Inventory.xlsx 文件，该文件将保存到项目的 \bin\Debug\net5.0 文件夹中。

虽然前面的代码似乎没有使用任何动态数据，但请注意 DLR 提供了很大的帮助。如果没有 DLR，代码将不得不写成这样：

```
static void ExportToExcelManual(List<Car> carsInStock)
{
    Excel.Application excelApp = new Excel.Application();
    // Must mark missing params!
    excelApp.Workbooks.Add(Type.Missing);
    // Must cast Object as _Worksheet!
    Excel._Worksheet workSheet =
        (Excel._Worksheet)excelApp.ActiveSheet;
    // Must cast each Object as Range object then call low-level Value2 property!
    ((Excel.Range)excelApp.Cells[1, "A"]).Value2 = "Make";
    ((Excel.Range)excelApp.Cells[1, "B"]).Value2 = "Color";
    ((Excel.Range)excelApp.Cells[1, "C"]).Value2 = "Pet Name";
    int row = 1;
    foreach (Car c in carsInStock)
    {
        row++;
        // Must cast each Object as Range and call low-level Value2 prop!
        ((Excel.Range)workSheet.Cells[row, "A"]).Value2 = c.Make;
        ((Excel.Range)workSheet.Cells[row, "B"]).Value2 = c.Color;
        ((Excel.Range)workSheet.Cells[row, "C"]).Value2 = c.PetName;
    }
    // Must call get_Range method and then specify all missing args!
    excelApp.get_Range("A1", Type.Missing).AutoFormat(
        Excel.XlRangeAutoFormat.xlRangeAutoFormatClassic2,
        Type.Missing, Type.Missing, Type.Missing,

```
Type.Missing, Type.Missing, Type.Missing);
// Must specify all missing optional args!
workSheet.SaveAs(
    $@"{Environment.CurrentDirectory}\InventoryManual.xlsx",
    Type.Missing, Type.Missing, Type.Missing,
    Type.Missing, Type.Missing, Type.Missing,
    Type.Missing, Type.Missing, Type.Missing);
excelApp.Quit();
Console.WriteLine("The InventoryManual.xlsx file has been saved to your app folder");
}
```

这就结束了你对 C# dynamic 关键字和 DLR 的介绍。我希望你能看到这些特性如何简化复杂的编程任务，以及（也许更重要的是）理解其中的权衡。当你选择使用 dynamic 数据时，你会失去大量类型安全，你的代码库更容易出现运行时错误。

虽然关于 DLR 还有很多要说，但本章试图专注于在日常编程中实用且有用的主题。如果你想了解有关动态语言运行时的高级特性，例如与脚本语言的集成，请务必查阅 .NET Core SDK 文档（查找主题“Dynamic Language Runtime Overview”以开始）。

## Summary

dynamic 关键字允许你定义在运行时才知道其身份的数据。当由动态语言运行时处理时，自动创建的“表达式树”将被传递给正确的动态语言绑定器，在那里有效载荷将被解包并发送到正确的对象成员。

使用 dynamic 数据和 DLR，复杂的 C# 编程任务可以被极大地简化，尤其是将 COM 库合并到你的 .NET Core 应用程序中的操作。正如本章所示，这为 COM 互操作提供了许多进一步的简化（与 dynamic 数据无关），例如将 COM 互操作数据嵌入到你的应用程序中、可选参数和命名参数。

虽然这些特性确实可以简化你的代码，但请始终记住 dynamic 数据会使你的 C# 代码类型安全大大降低且容易出现运行时错误。务必权衡在你的 C# 项目中使用 dynamic 数据的利弊，并相应地进行测试！

## Understanding CIL and the Role of Dynamic Assemblies

当你构建一个全功能的 .NET Core 应用程序时，你肯定会使用 C#（或类似的托管语言，如 Visual Basic），鉴于其固有的生产力和易用性。然而，正如你在本书开头学到的那样，托管编译器的作用是将 *.cs 代码文件翻译成 CIL 代码、类型元数据和程序集清单的术语。事实证明，CIL 是一种功能齐全的 .NET Core 编程语言，具有自己的语法、语义和编译器（ilasm.exe）。

在本章中，你将获得 .NET Core 母语的导览。在这里，你将了解 CIL 指令、CIL 属性和 CIL 操作码之间的区别。然后你将学习 .NET Core 程序集的往返工程的作用以及各种 CIL 编程工具。随后，本章将带你了解使用 CIL 语法定义命名空间、类型和成员的基础知识。本章将以对 System.Reflection.Emit 命名空间的作用的考察作为结尾，并解释如何在运行时动态构造一个程序集（带有 CIL 指令）。

当然，很少有程序员需要在日常工作中处理原始 CIL 代码。因此，我将首先通过检查几个原因来开始本章，了解这种低级 .NET Core 语言的语法和语义可能值得你花费时间。

## Motivations for Learning the Grammar of CIL

CIL 是 .NET Core 平台真正的母语。当你使用你选择的托管语言（C#、VB、F# 等）构建 .NET Core 程序集时，相关编译器会将你的源代码翻译成 CIL 的术语。像任何编程语言一样，CIL 提供了许多结构化和以实现为中心的标记。鉴于 CIL 只是一种 .NET Core 编程语言，应该不会让人感到惊讶的是，可以直接使用 CIL 和 CIL 编译器（ilasm.exe）构建你的 .NET Core 程序集。

注意 如第 1 章所述，ildasm.exe 和 ilasm.exe 都不随 .NET 5 运行时一起提供。有两种获取这些工具的方法。第一种是从 https://github.com/dotnet/runtime 上的源代码编译 .NET 5 运行时。第二种、更简单的方法是从 www.nuget.org 下载所需版本。ILDasm 在 NuGet 上的 URL 为 https://www.nuget.org/packages/Microsoft.NETCore.ILDAsm/，ILAsm.exe 为 https://www.nuget.org/packages/Microsoft.NETCore.ILAsm/。请确保选择正确的版本（对于本书你需要 5.0.0 或更高版本）。使用以下命令将 ILDasm 和 ILAsm NuGet 包添加到你的项目中：

