
# 10.2 Perform an XPath Query Against XML

## Problem

You want to perform an advanced query against an XML file using XML’s standard `XPath syntax`.

## Solution

Use PowerShell’s `Select-Xml` cmdlet to perform an XPath query against a file.

For example, to find all post titles shorter than 30 characters in an RSS feed:

```
PS > $query = "/rss/channel/item[string-length(title) < 30]/title"
PS > Select-Xml -XPath $query -Path .\powershell_blog.xml | Select -Expand Node

#text
-----
Remoting for non-Admins
```

## Discussion

Although a language all of its own, the XPath query syntax provides a powerful, XML-centric way to write advanced queries for XML files. The `Select-Xml` cmdlet lets you apply these concepts to files, XML nodes, or simply plain text.

### Note

The XPath queries supported by the `Select-Xml` cmdlet are a popular industry standard. Beware, though. Unlike those in the rest of PowerShell, these queries are case-sensitive!

The `Select-Xml` cmdlet generates a `SelectXmlInfo` object. This lets you chain separate XPath queries together. To retrieve the actual result of the selection, access the `Node` property:

```
PS > Get-Content page.html
<HTML>
    <HEAD>
        <TITLE>Welcome to my Website</TITLE>
    </HEAD>
    <BODY>
        <P>...</P>
    </BODY>
</HTML>
PS > $content = [xml] (Get-Content page.html)
PS > $result = $content | Select-Xml "/HTML/HEAD" | Select-Xml "TITLE"
PS > $result

Node                     Path                     Pattern
----                     ----                     -------
TITLE                    InputStream              TITLE


PS > $result.Node

#text
-----
Welcome to my Website
```

This works even for content accessed through PowerShell’s XML support, as in this case, which uses the RSS feed downloaded from the PowerShell blog:

```
PS > $xml = [xml] (Get-Content powershell_blog.xml)
PS > $xml | Select-Xml $query | Select -Expand Node

#text
-----
Remoting for non-Admins
```

For simpler queries, you may find PowerShell’s object-based XML navigation concepts easier to work with. For more information about working with XML through PowerShell’s XML type, see [Table F-11](#app06.xhtml#xml-id001) in [Appendix F](#app06.xhtml#selected_net_classes_and_their_uses). For more information about XPath syntax, see [Appendix C](#app03.xhtml#xpath_reference).

## See Also

- [Appendix C, *XPath Quick Reference*](#app03.xhtml#xpath_reference)
- [Table F-11](#app06.xhtml#xml-id001)

# 10.3 Convert Objects to XML

## Problem

You want to convert command output to XML for further processing or viewing.

## Solution

Use PowerShell’s `ConvertTo-Xml` cmdlet to save the output of a command as XML:

```
$xml = Get-Process | ConvertTo-Xml
```

You can then use PowerShell’s XML support (XML navigation, `Select-Xml`, and more) to work with the content.

## Discussion

Although it’s usually easiest to work with objects in their full fidelity, you may sometimes want to convert them to XML for further processing by other programs. The solution is the `ConvertTo-Xml` cmdlet.

### Note

PowerShell includes another similar-sounding cmdlet called `Export-CliXml`. Unlike the `ConvertTo-Xml` cmdlet, which is intended to produce useful output for humans and programs alike, the `Export-CliXml` cmdlet is designed for PowerShell-centric data interchange. For more information, see [Recipe 10.5](#ch10.xhtml#easily_import_and_export_your_structured).

The `ConvertTo-Xml` cmdlet gives you two main targets for this conversion. The default is an XML document, which is the same type of object created by the `[xml]` cast in PowerShell. This is also the format supported by the `Select-Xml` cmdlet, so you can pipe the output of `ConvertTo-Xml` directly into it.

```
PS > $xml = Get-Process | ConvertTo-Xml
PS > $xml | Select-Xml '//Property[@Name = "Name"]' | Select -Expand Node

Name                     Type                     #text
----                     ----                     -----
Name                     System.String            audiodg
Name                     System.String            csrss
Name                     System.String            dwm
(...)
```

The second format is a simple string, and it’s suitable for redirection into a file. To save the XML into a file, use the `-As` parameter with `String` as the argument, and then use the file redirection operator:

```
Get-Process | ConvertTo-Xml -As String > c:\temp\processes.xml
```

If you already have an XML document that you obtained from `ConvertTo-Xml` or PowerShell’s `[xml]` cast, you can still save it into a file by calling its `Save()` method:

```
$xml = Get-Process | ConvertTo-Xml
$xml.Save("c:\temp\output.xml")
```

For more information on how to work with XML data in PowerShell, see [Recipe 10.1](#ch10.xhtml#access_information_in_an_xml_file).

## See Also

- [Recipe 10.1, “Access Information in an XML File”](#ch10.xhtml#access_information_in_an_xml_file)
- [Recipe 10.5, “Easily Import and Export Your Structured Data”](#ch10.xhtml#easily_import_and_export_your_structured)

# 10.4 Modify Data in an XML File

## Problem

You want to use PowerShell to modify the data in an XML file.

## Solution

To modify data in an XML file, load the file into PowerShell’s XML data type, change the content you want, and then save the file back to disk. [Example 10-4](#ch10.xhtml#modifying_an_xml_file_from_powershell) demonstrates this approach.

### Example 10-4. Modifying an XML file from PowerShell

```
PS > ## Store the filename
PS > $filename = (Get-Item phone.xml).FullName
PS >
PS > ## Get the content of the file, and load it
PS > ## as XML
PS > Get-Content $filename
<AddressBook>
  <Person contactType="Personal">
    <Name>Lee</Name>
    <Phone type="home">555-1212</Phone>
    <Phone type="work">555-1213</Phone>
  </Person>
  <Person contactType="Business">
    <Name>Ariel</Name>
    <Phone>555-1234</Phone>
  </Person>
</AddressBook>
PS > $phoneBook = [xml] (Get-Content $filename)
PS >
PS > ## Get the part with data we want to change
PS > $person = $phoneBook.AddressBook.Person[0]
PS >
PS > ## Change the text part of the information,
PS > ## and the type (which was an attribute)
PS > $person.Phone[0]."#text" = "555-1214"
PS > $person.Phone[0].type = "mobile"
PS >
PS > ## Add a new phone entry
PS > $newNumber = [xml] '<Phone type="home">555-1215</Phone>'
PS > $newNode = $phoneBook.ImportNode($newNumber.Phone, $true)
PS > [void] $person.AppendChild($newNode)
PS >
PS > ## Save the file to disk
PS > $phoneBook.Save($filename)
PS > Get-Content $filename
<AddressBook>
  <Person contactType="Personal">
    <Name>Lee</Name>
    <Phone type="mobile">555-1214</Phone>
    <Phone type="work">555-1213</Phone>
    <Phone type="home">555-1215</Phone>
  </Person>
  <Person contactType="Business">
    <Name>Ariel</Name>
    <Phone>555-1234</Phone>
  </Person>
</AddressBook>
```

## Discussion

In the preceding Solution, you change `Lee`’s phone number (which was the “text” portion of the XML’s original first `Phone` node) from `555-1212` to `555-1214`. You also change the type of the phone number (which was an attribute of the `Phone` node) from `"home"` to `"mobile"`.

Adding new information to the XML is nearly as easy. To add information to an XML file, you need to add it as a *child node* to another node in the file. The easiest way to get that child node is to write the string that represents the XML and then create a temporary PowerShell XML document from that. From that temporary document, you use the main XML document’s `ImportNode()` function to import the node you care about—specifically, the `Phone` node in this example.

---

### Cross-References

- [Recipe 10.2, “Perform an XPath Query Against XML”](#ch10.xhtml#perform_an_xpath_query_against_an_xml_fi)
- [Recipe 10.4, “Modify Data in an XML File”](#ch10.xhtml#modify_data_in_an_xml_file)
- [Recipe 12.1, “Download a File from an FTP or Internet Site”](#ch12.xhtml#download_a_file_from_the_internet)
- [Table F-11](#app06.xhtml#xml-id001)
