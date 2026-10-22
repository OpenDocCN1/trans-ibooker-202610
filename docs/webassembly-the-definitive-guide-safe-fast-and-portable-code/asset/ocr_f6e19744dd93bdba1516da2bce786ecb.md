```
bodyContent += "\n";
request.respond({ status: 200, body: bodyContent });

// Close the Database
db.close();
}
```

We start by creating the database file. I hope your intuition kicked in that this is going to require another runtime permission, because it does as you see below. After that, we load some data into the database and close out the connection.

Once we start up the server, upon receiving a suitable HTTP request, we will open up the database again, run a query, generate a result and then close the database. I am not suggesting that this is quality production code yet, but it is quite remarkable that this code runs securely in Deno and will do so across the various platforms that Deno supports. Even though we are dealing with a wrapped C library (i.e. SQLite3), WebAssembly makes the code portable while still being fairly performant. I hope the idea of extending server infrastructure with safe, fast and portable WebAssembly code makes more sense.

The following command will start up the server with suitable permissions.

```
brian@tweezer ~/g/w/s/c/deno> deno run --allow-read --allow-write
--allow-net db-serve.ts
HTTP webserver running. Access it at: http://localhost:9000/
```

A request from an HTTP client produces what we would expect.

```
brian@tweezer ~> http http://localhost:9000
HTTP/1.1 200 OK
content-length: 74

Programming Languages that work with WebAssembly:

C
C++
Rust
TypeScript
```