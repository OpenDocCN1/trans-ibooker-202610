```
"IsVinyl":true
}';
SELECT * FROM OPENJSON(@json);
```

Of course, the optimizer does not forgive this small mistake and its reaction is very conservative:

```
Msg 13609, Level 16, State 4, Line 23
JSON text is not properly formatted. Unexpected character 'Y' is found at position 34.
```

As already mentioned and demonstrated, only first-level properties are returned with the OPENJSON function. To return properties within complex values of a JSON document (arrays and objects), you need to specify the path argument. In this example, assume you want to return the Songs fragment from the initial Wish You Were Here JSON string:

```
DECLARE @json NVARCHAR(MAX) = N'{
"Album":"Wish You Were Here",
"Year":1975,
"IsVinyl":true,
"Songs":[{"Title":"Shine On You Crazy Diamond","Authors":"Gilmour, Waters, Wright"},
{"Title":"Have a Cigar","Authors":"Waters"},
{"Title":"Welcome to the Machine","Authors":"Waters"},
{"Title":"Wish You Were Here","Authors":"Gilmour, Waters"}],
"Members":{"Guitar":"David Gilmour","Bass Guitar":"Roger Waters","Keyboard":"Richard Wright","Drums":"Nick Mason"}
}';
SELECT * FROM OPENJSON(@json, '$.Songs');
```

The $ path expression represents the context item and $.Songs refers to the Songs property and actually extracts this fragment from the JSON document. The rest of the document must be valid; otherwise, the path expression cannot be evaluated.

Here is the result:

| key | value | type |
| --- | --- | --- |
| 0 | {"Title":"Shine On You Crazy Diamond","Writers":"Gilmour, Waters, Wright"} | 5 |
| 1 | {"Title":"Have a Cigar","Writers":"Waters"} | 5 |
| 2 | {"Title":"Welcome to the Machine","Writers":"Waters"} | 5 |
| 3 | {"Title":"Wish You Were Here","Writers":"Gilmour, Waters"} | 5 |