# Database CRUD

While we could use a basic approach of storing the list of article titles and their unique web link identifiers to a plain-text file, we would then need to be responsible for the code to parse and update that file. As the file grew larger with more entries, the parsing would become slower due to more data to iterate through. And if we need to delete any data from the list, we would also need to maintain the integrity of the rest of the list via a lot of error-checking code.

Fortunately for our project, Python includes built-in support for a rather basic yet powerful and extremely popular database engine called SQLite.[57] While not nearly as feature-packed as other open source database engines like PostgreSQL,[58] SQLite is ideal for projects like ours that just need a local data store to perform create, read, update, and delete (better known as CRUD) operations. SQLite is lightweight and fast, and it can be called upon in Python with just a few simple lines of code.

The SQLite engine is already baked into Python, but it will be useful for us to view the data stored in the database we create from our Python script. A number of ways to accomplish this exist, with the most simple being to install the sqlite3 engine for Raspberry Pi OS from the Terminal:

```
$ sudo apt install sqlite3
```

Once installed, you can call upon this engine by entering sqlite3 at the Terminal’s prompt. This will launch SQLite and present you with its own prompt. Learning the various commands SQLite has to offer is beyond the scope of this book, but I strongly recommend learning more about SQLite from websites such as SQL Tutorial.[59]

If you’re running the Raspberry Pi OS Desktop edition, a helpful GUI-based SQLite database client is DB Browser for SQLite. You can install this using