# Data Formats

As mentioned earlier in the chapter, the API is the action that is performed on data, but the data needs to be in a format that facilitates it being shared between platforms, applications, and devices. This is where two common data formats, also known as data encoding methods, come into play: XML and JSON.

# XML Data Format

The Extensible Markup Language (XML) is a common format for data that needs to be exchanged so that the data is understood at the destination. XML got really popular in the early 2000s and is still used today to exchange data.

XML is easy to read because it uses tags, similar to HTML tags, to describe the data. Looking at the following XML code, you can see that there are multiple books in the data, with the title of the first book being CompTIA Security+ Certification Study Guide and the second book being CompTIA Network+ Certification Study Guide:

```
<Books>
    <Book ISBN="1260026051">
        <Title>CompTIA Security+ Certification Study Guide</Title>
        <Author>Glen E. Clarke</Author>
        <Price>34.76</Price>
    </Book>
    <Book ISBN="1260122042">
        <Title>CompTIA Network+ Certification Study Guide </Title>
        <Author>Glen E. Clarke</Author>
        <Price>37.26</Price>
    </Book>
</Books>
```

Notice that it is very clear that the author of both books is Glen E. Clarke. The use of the tags ensures that the receiver of the data does not misinterpret the information (in other words, the receiver will not confuse author Glen E. Clarke with the titles of the books).