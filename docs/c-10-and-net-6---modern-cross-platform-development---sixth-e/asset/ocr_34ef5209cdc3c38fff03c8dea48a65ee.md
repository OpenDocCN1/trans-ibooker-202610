Let's suppress the warning by modifying the stylecop.json file:

+   1. In stylecop.json, add a configuration option to set using statements to be allowable outside a namespace, as shown highlighted in the following markup:
+   2. Build the project and note that warning SA1200 has disappeared.
+   3. In stylecop.json, set the using directives placement to preserve, which allows using statements both inside and outside a namespace, as shown in the following markup:

```
{
  "$schema": "https://raw.githubusercontent.com/DotNetAnalyzers/StyleCopAnalyzers/master/StyleCop.Analyzers/StyleCop.Analyzers/Settings/stylecop.schema.json",
  "settings": {
    "orderingRules": {
      "usingDirectivesPlacement": "outsideNamespace"
    }
  }
}
```

```
"orderingRules": {
  "usingDirectivesPlacement": "preserve"
}
```

# Fixing the code

Now, let's fix all the other warnings:

+   1. In CodeAnalyzing.csproj, add an element to automatically generate an XML file for documentation, as shown highlighted in the following markup:
+   2. In stylecop.json, add a configuration option to provide values for documentation for the company name and copyright text, as shown highlighted in the following markup:

```
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net6.0</TargetFramework>
    <GenerateDocumentationFile>true</GenerateDocumentationFile>
  </PropertyGroup>
</Project>
```

```
{
  "$schema": "https://raw.githubusercontent.com/DotNetAnalyzers/StyleCopAnalyzers/master/StyleCop.Analyzers/StyleCop.Analyzers/Settings/stylecop.schema.json",
  "settings": {
```