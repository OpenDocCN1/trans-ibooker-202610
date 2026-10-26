4. Add the following private constant declarations to the end of the current list of constants:

```
private const String REGEX_OPTIONS_PROPERTY_NAME = "Regular Expressions Options";
private const String REGEX_OPTIONS_PROPERTY_DESCRIPTION = "The Regular Expressions options to be used in email address validation.";
private const Int64 REGEX_OPTIONS_UPGRADE_VALUE = 513;
private const String REGEX_OPTIONS_UNKNOWN_MESSAGE_PATTERN = "The value of {0} does not represent a valid RegexOptions value.";
```

These constants contain some of the settings needed by version two of the component: object names, descriptions, and messages.

5. Another private variable is required; place it next to the existing ones:

```
private Int64 _regexOptionsNumber;
```

6. Modify the ProvideComponentProperties() method, and add the following new custom property declaration:

```
// Regular Expressions Options
IDTSCustomProperty100 regularExpressionsOptions = ComponentMetaData.CustomPropertyCollection.New();
regularExpressionsOptions.Name = REGEX_OPTIONS_PROPERTY_NAME;
regularExpressionsOptions.Description = REGEX_OPTIONS_PROPERTY_DESCRIPTION;
regularExpressionsOptions.State = DTSPersistState.PS_DEFAULT;
regularExpressionsOptions.TypeConverter = typeof(Int64).AssemblyQualifiedName;
regularExpressionsOptions.Value = (Int64)0;
```

This property will allow the SSIS package developers to not only specify which regular expressions patterns to use, and against which column, but also which regular expressions options to use for matching.