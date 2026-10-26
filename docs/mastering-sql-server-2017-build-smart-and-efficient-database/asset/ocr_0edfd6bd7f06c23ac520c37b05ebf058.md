```
throw new NotSupportedException(TOO_MANY_INPUTS_MESSAGE);
}
```

This method will prevent the SSIS developers from creating any additional inputs:

```
public override IDTSOutput100 InsertOutput(DTSInsertPlacement insertPlacement, Int32 outputID)
{
    // Only one output is supported.
    throw new NotSupportedException(TOO_MANY_OUTPUTS_MESSAGE);
}
```

This method will prevent the SSIS developers from creating any additional outputs:

```
public override IDTSOutputColumn100 InsertOutputColumnAt(Int32 outputID, Int32 outputColumnIndex, String name, String description)
{
    // No additional Output Columns can be added.
    throw new NotSupportedException(USER_DEFINED_COLUMNS_NOT_SUPPORTED);
}
```

This method will prevent the SSIS developers from adding any additional columns to the default output:

```
public override void DeleteOutput(Int32 outputID)
{
    // The built-in output cannot be removed.
    Boolean isInternal =
    (Boolean)(ComponentMetaData.OutputCollection.GetObjectByID(outputID).CustomPropertyCollection[IS_INTERNAL_OBJECT_PROPERTY_NAME].Value);
    if (isInternal)
    {
        throw new InvalidOperationException(DEFAULT_OUTPUT_MUST_EXIST_MESSAGE);
    }
    else
    {
        base.DeleteOutput(outputID);
    }
}
```