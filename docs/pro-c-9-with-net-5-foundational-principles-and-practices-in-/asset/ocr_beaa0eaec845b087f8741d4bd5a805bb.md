# Working with Pointer Types

And now for the final topic of the chapter, which most likely will be the least used of all C# features for most of your .NET Core projects.

Note In the examples that follow, I'm assuming you have some background in C++ pointer manipulation. If this is not true, feel free to skip this topic entirely. Using pointers will not be a common task for most C# applications.

In Chapter 4, you learned that the .NET Core platform defines two major categories of data: value types and reference types. Truth be told, however, there is a third category: pointer types. To work with pointer types, you get specific operators and keywords that allow you to bypass the .Net 5 Runtime's memory management scheme and take matters into your own hands (see Table 11-2).

# Table 11-2. Pointer-centric C# Operators and Keywords

| Operator/Keyword | Meaning in Life |
|---|---|
| * | This operator is used to create a pointer variable (i.e., a variable that represents a direct location in memory). As in C++, this same operator is used for pointer indirection. |
| & | This operator is used to obtain the address of a variable in memory. |
| -> | This operator is used to access fields of a type that is represented by a pointer (the unsafe version of the C# dot operator). |
| [] | This operator (in an unsafe context) allows you to index the slot pointed to by a pointer variable (if you're a C++ programmer, you will recall the interplay between a pointer variable and the [] operator). |
| ++, -- | In an unsafe context, the increment and decrement operators can be applied to pointer types. |
| +, - | In an unsafe context, the addition and subtraction operators can be applied to pointer types. |
| ==, !=, <, >, <=, >= | In an unsafe context, the comparison and equality operators can be applied to pointer types. |
| Stackalloc | In an unsafe context, the stackalloc keyword can be used to allocate C# arrays directly on the stack. |
| Fixed | In an unsafe context, the fixed keyword can be used to temporarily fix a variable so that its address can be found. |

Now, before digging into the details, let me again point out that you will seldom if ever need to make use of pointer types. Although C# does allow you to drop down to the level of pointer manipulations, understand that the .NET Core runtime has absolutely no clue of your intentions. Thus, if you mismanage a pointer, you are the one in charge of dealing with the consequences. Given these warnings, when exactly would you need to work with pointer types? There are two common situations.