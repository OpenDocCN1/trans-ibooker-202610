```
IL_0007: ldloc.0
IL_0008: ret
} // end of method Calc::Add
```

Don’t worry if you are unable to make heads or tails of the resulting CIL for this method because Chapter 19 will describe the basics of the CIL programming language. The point to concentrate on is that the C# compiler emits CIL, not platform-specific instructions.

Now, recall that this is true of all .NET Core compilers. To illustrate, assume you created this same application using Visual Basic, rather than C#.

```
' Calc.vb
Namespace CalculatorExample
    Module Program
        ' This class contains the app's entry point.
        Sub Main(args As String())
            Dim c As New Calc
            Dim ans As Integer = c.Add(10, 84)
            Console.WriteLine("10 + 84 is {0}", ans)
            'Wait for user to press the Enter key before shutting down
            Console.ReadLine()
        End Sub
    End Module
    ' The VB.NET calculator.
    Class Calc
        Public Function Add(ByVal addend1 As Integer, ByVal addend2 As Integer) As Integer
            Return addend1 + addend2
        End Function
    End Class
End Namespace
```

If you examine the CIL for the Add() method, you find similar instructions (slightly tweaked by the Visual Basic compiler).

```
.method public instance int32 Add(int32 addend1,
    int32 addend2) cil managed
{
    // Code size       9 (0x9)
    .maxstack 2
    .locals init (int32 V_0)
    IL_0000: nop
    IL_0001: ldarg.1
    IL_0002: ldarg.2
    IL_0003: add.ovf
    IL_0004: stloc.0
    IL_0005: br.s       IL_0007

    IL_0007: ldloc.0
    IL_0008: ret
} // end of method Calc::Add
```