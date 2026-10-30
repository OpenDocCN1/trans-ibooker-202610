# Working with String Data

System.String provides a number of methods you would expect from such a utility class, including methods that return the length of the character data, find substrings within the current string, and convert to and from uppercase/lowercase. Table 3-5 lists some (but by no means all) of the interesting members.

Table 3-5. Select Members of System.String

| String Member | Meaning in Life |
|---|---|
| Length | This property returns the length of the current string. |
| Compare() | This static method compares two strings. |
| Contains() | This method determines whether a string contains a specific substring. |
| Equals() | This method tests whether two string objects contain identical character data. |
| Format() | This static method formats a string using other primitives (e.g., numerical data, other strings) and the {0} notation examined earlier in this chapter. |
| Insert() | This method inserts a string within a given string. |
| PadLeft() \ PadRight() | These methods are used to pad a string with some characters. |
| Remove() \ Replace() | These methods are used to receive a copy of a string with modifications (characters removed or replaced). |
| Split() | This method returns a String array containing the substrings in this instance that are delimited by elements of a specified char array or string array. |
| Trim() | This method removes all occurrences of a set of specified characters from the beginning and end of the current string. |
| ToUpper() \ ToLower() | These methods create a copy of the current string in uppercase or lowercase format, respectively. |

# Performing Basic String Manipulation

Working with the members of System.String is as you would expect. Simply declare a string variable and make use of the provided functionality via the dot operator. Be aware that a few of the members of System.String are static members and are, therefore, called at the class (rather than the object) level.

Assume you have created a new Console Application project named FunWithStrings and added it to your solution. Clear out the existing code and add the following:

```
using System;
using System.Text;
BasicStringFunctionality();

static void BasicStringFunctionality()
{
Console.WriteLine("=> Basic String functionality:");
string firstName = "Freddy";
Console.WriteLine("Value of firstName: {0}", firstName);
Console.WriteLine("firstName has {0} characters.", firstName.Length);
Console.WriteLine("firstName in uppercase: {0}", firstName.ToUpper());
Console.WriteLine("firstName in lowercase: {0}", firstName.ToLower());
```