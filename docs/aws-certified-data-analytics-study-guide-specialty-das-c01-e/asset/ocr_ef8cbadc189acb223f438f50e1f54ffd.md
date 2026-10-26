# TABLE 3.3 Scalar data types in DynamoDB

| Data Type | Description | Range |
|---|---|---|
| Number | Numbers can be positive, negative, or zero. Numbers are represented as variable length. You can also use the number data type to represent a date or a time stamp using epoch time or any other representation of time. All numbers are sent across the network to DynamoDB as strings, to maximize compatibility across languages and libraries. | Numbers can have up to 38 digits of precision. |
| String | Strings are Unicode with UTF-8 encoding. | Length of a string must be greater than zero up to a maximum of 400 KB (DynamoDB item size limit).
For a simple primary key, the maximum length of the first attribute value (the partition key) is 2048 bytes.
For a composite primary key, the maximum length of the second attribute value (the sort key) is 1024 bytes. |
| Binary | Binary type attributes can store any binary data such as compressed text, encrypted data, or images. During the comparison of binary values in DynamoDB, each byte is treated as unsigned. Your applications must encode binary values in base64-encoded format before sending them to DynamoDB. Upon receipt of these values, DynamoDB decodes the data into an unsigned byte array and uses that as the length of the binary attribute. | Length should be greater than 0 up to 400 KB (DynamoDB item size limit). |
| Boolean | Boolean data type is a data type that has one of two possible values (usually denoted true and false), which is intended to represent the two truth values of logic and Boolean algebra. | A Boolean type attribute can be stored as either “true” or “false.” |
| Null | In databases, a Null value represents absence of a value. A Null value is different from the number zero. | Null represents an unknown or undefined state. |