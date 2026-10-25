The advantage of text files is that they conform to a relatively universal and simple format—lines of text separated by newlines—while binary files have no universally recognized format. Yet the latter has advantages in terms of performance.

> performance Tip
> If a data file has a large amount of data and if it’s all numeric, then programs that use binary format to deal with it (as opposed to text format, the default) can frequently run several times faster. That’s because they spend no time on costly numeric-to-text or text-to numeric conversions.

# ## 8.1.2 Binary Files

A binary file can contain printable data, but it doesn’t have to. The biggest difference occurs when you read and write numbers.

As shown in Figure 8.2, text-file operations write out all data as human-readable characters, including numerals (that is, they are written out as decimal characters). So the number 1,000 is written out as the character “1” followed by three “0” characters.

| 1 | , | 0 | 0 | 0 | (sp) | - | 1 | 0 | (sp) |
|---|---|---|---|---|---|---|---|---|---|

Text File: each byte contains a character.

| 1000 | -10 |
|---|---|

Binary File: structured into fixed-length interger fields.

**Figure 8.2.** Text versus binary operations