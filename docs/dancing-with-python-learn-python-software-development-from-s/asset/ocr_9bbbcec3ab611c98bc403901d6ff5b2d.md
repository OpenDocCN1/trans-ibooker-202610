```
"nothing": null,
"pi.approximation": 3.14159,
"python.formatting.autopep8Args": [
    "--max-line-length",
    "100"
]
}
```

I used the indent and sort_keys optional arguments to make the JSON expression more readable.

Exercise 8.18
Python True, False, and None correspond to which JSON values?

Now that I have the data in JSON format, I can write it to a text file in the usual way. I can also dump it directly into a file.

```
with open("work/new_example.json", 'w') as output_file:
    json.dump(json_, output_file, indent=4, sort_keys=True)
```

# 8.9 Summary

We use files to store data in information for later reuse. In this chapter, we looked at Python functions for moving around and examining the file system controlled by the computer’s operating system. We learned that files come in two varieties, text and binary. We saw how to access the files and read and write to them for each variety.

Other than pickling and using JSON, you can use a database to save and retrieve information, though we do not cover that here. We explore working with datasets with pandas in Chapter 14, Analyzing Data.

We have now completed Part I and the fundamentals of Python. We now examine and implement classical and quantum algorithms and circuits in Part II.