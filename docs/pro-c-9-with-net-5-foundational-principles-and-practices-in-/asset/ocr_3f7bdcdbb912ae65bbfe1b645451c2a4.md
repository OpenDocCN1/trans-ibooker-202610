```
<!-- The Type markup extension is a XAML version of the C# typeof operator -->
<Label Content ="{x:Type Button}" />
<Label Content ="{x:Type CorLib:Boolean}" />

<!-- Fill a ListBox with an array of strings! -->
<ListBox Width="200" Height="50">
    <ListBox.ItemsSource>
        <x:Array Type="CorLib:String">
            <CorLib:String>Sun Kil Moon</CorLib:String>
            <CorLib:String>Red House Painters</CorLib:String>
            <CorLib:String>Besnard Lakes</CorLib:String>
        </x:Array>
    </ListBox.ItemsSource>
</ListBox>
</StackPanel>
</Page>
```

First, notice that the <Page> definition has a new XML namespace declaration, which allows you to gain access to the System namespace of mscorlib.dll. With this XML namespace established, you first make use of the x:Static markup extension and grab values from OSVersion and ProcessorCount of the System.Environment class.

The x:Type markup extension allows you to gain access to the metadata description of the specified item. Here, you are simply assigning the fully qualified names of the WPF Button and System.Boolean types.

The most interesting part of this markup is the ListBox. Here, you are setting the ItemsSource property to an array of strings declared entirely in markup! Notice here how the x:Array markup extension allows you to specify a set of subitems within its scope:

```
<x:Array Type="CorLib:String">
    <CorLib:String>Sun Kil Moon</CorLib:String>
    <CorLib:String>Red House Painters</CorLib:String>
    <CorLib:String>Besnard Lakes</CorLib:String>
</x:Array>
```

Note The previous XAML example is used only to illustrate a markup extension in action. As you will see in Chapter 25, there are much easier ways to populate ListBox controls!

Figure 24-3 shows the markup of this <Page> in Kaxaml.