```
<name>CodeAnalyzing</name>
</assembly>
<members>
    <member name="T:CodeAnalyzing.Program">
        <summary>
        The main class for this console app.
        </summary>
    </member>
    <member name="M:CodeAnalyzing.Program.Main(System.String[])">
        <summary>
        The main entry point for this console app.
        </summary>
        <param name="args">A string array of arguments passed to the console app.</param>
    </member>
</members>
</doc>
```

# Understanding common StyleCop recommendations

Inside a code file, you should order the contents, as shown in the following list:

+   1. External alias directives
+   2. Using directives
+   3. Namespaces
+   4. Delegates
+   5. Enums
+   6. Interfaces
+   7. Structs
+   8. Classes

Within a class, record, struct, or interface, you should order the contents, as shown in the following list:

+   1. Fields
+   2. Constructors
+   3. Destructors (finalizers)
+   4. Delegates
+   5. Events
+   6. Enums
+   7. Interfaces
+   8. Properties
+   9. Indexers
+   10. Methods