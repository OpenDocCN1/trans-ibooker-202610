2. Near the top of Program.cs, call WorkWithCompression twice, once with the default using Brotli and once with GZIP, as shown in the following code:

```
```
WorkWithCompression();
WorkWithCompression(useBrotli: false);
```
```

3. Run the code and compare the sizes of the two compressed XML files. Brotli is more than 21% denser, as shown in the following edited output:

```
```
/Users/markjprice/Code/Chapter09/WorkingWithStreams/streams.brotli contains 118 bytes.
/Users/markjprice/Code/Chapter09/WorkingWithStreams/streams.gz contains 150 bytes.
```
```

# Encoding and decoding text

Text characters can be represented in different ways. For example, the alphabet can be encoded using Morse code into a series of dots and dashes for transmission over a telegraph line.

In a similar way, text inside a computer is stored as bits (ones and zeros) representing a code point within a code space. Most code points represent a single character, but they can also have other meanings like formatting.

For example, ASCII has a code space with 128 code points. .NET uses a standard called Unicode to encode text internally. Unicode has more than one million code points.

Sometimes, you will need to move text outside .NET for use by systems that do not use Unicode or use a variation of Unicode, so it is important to learn how to convert between encodings.

The following table lists some alternative text encodings commonly used by computers:

| Encoding | Description |
|---|---|
| ASCII | This encodes a limited range of characters using the lower seven bits of a byte. |
| UTF-8 | This represents each Unicode code point as a sequence of one to four bytes. |
| UTF-7 | This is designed to be more efficient over 7-bit channels than UTF-8 but it has security and robustness issues, so UTF-8 is recommended over UTF-7. |
| UTF-16 | This represents each Unicode code point as a sequence of one or two 16-bit integers. |
| UTF-32 | This represents each Unicode code point as a 32-bit integer and is therefore a fixed-length encoding unlike the other Unicode encodings, which are all variable-length encodings. |
| ANSI/ISO encodings | This provides support for a variety of code pages that are used to support a specific language or group of languages. |