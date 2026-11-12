```
<ProjectReference
Include="..\eBookManager.Controls\eBookManager.Controls.csproj" />
<ProjectReference
Include="..\eBookManager.Engine\eBookManager.Engine.csproj" />
<ProjectReference
Include="..\eBookManager.Helper\eBookManager.Helper.csproj" />
</ItemGroup>

<ItemGroup>
<Reference Include="System">
<HintPath>System</HintPath>
</Reference>
</ItemGroup>
</Project>
```

This is, essentially, all that's needed; however, you will need to decide what to do about the project's resources. You can manually just check that they are all set to copy to the output directory; alternatively, we can add an ItemGroup to the project file, such as the following:

```
<ItemGroup>
<None Update="ico\importBooks.ico">
<CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</None>
<None Update="ico\mainForm.ico">
<CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</None>
<None Update="img\add_ebook_to_storage_space.png">
<CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</None>
<None Update="img\add_new_storage_space.png">
<CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</None>
<None Update="img\docx16.png">
<CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</None>
<None Update="img\docxx16.png">
<CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</None>
<None Update="img\eBook.png">
<CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</None>
<None Update="img\epubx16.png">
<CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</None>
<None Update="img\folder-close-x16.png">
<CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</None>
<None Update="img\folder_exp_x16.png">
```