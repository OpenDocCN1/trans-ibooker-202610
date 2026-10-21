Additionally, the dvga-report will contain several folders, as shown here:

```
# ls -l dvga-report/
total 112
-rw-r--r-- 1 kali kali 95957 Dec 15 15:19 jquery.min.js
-rw-r--r-- 1 kali kali 2356 Feb 11 15:10 report.html
drwxr-xr-x 2 kali kali 4096 Feb 11 15:09 screens
drwxr-xr-x 2 kali kali 4096 Feb 11 15:09 source
-rw-r--r-- 1 kali kali 684 Feb 11 15:09 style.css
```

The report.html file includes information about the target, such as the HTTP response headers it sent back to the client, a screen capture of the application running on the target, and a link to the web page’s source code. While you can visually identify the GraphiQL IDE by using the screen capture taken by EyeWitness, you can also confirm your finding by searching the source folder, where the source code files reside. Run the command shown in Listing 4-14 to search for any GraphiQL Explorer or GraphQL Playground strings within the source code.

```
# grep -Hnio "graphiql|graphql-playground" dvga-report/source/*
source/http.localhost.5013.graphiql.txt:18:graphiql
source/http.localhost.5013.graphiql.txt:18:graphiql
source/http.localhost.5013.graphiql.txt:18:graphiql
```

Listing 4-14: Keyword matches in the web page source code

Let’s break down the command to explain what’s happening here. We run a case-insensitive search using grep by passing it the i flag to find any instances of the words graphql or graphql-playground in the source folder. Using the -H flag, we tell grep to print the names of files containing any pattern matches. The -n flag indicates the line number at which the match is located (in this case, 18). The -o flag prints only the parts of matching lines that yielded positive results. As you can see, the search found multiple instances of the string graphiql at line number 18.

EyeWitness can run the same type of scan against a list of URLs, as opposed to a single URL, using the -f (file) flag. When you use this flag, EyeWitness will expect a text file containing a list of target URLs to scan. Listing 4-15 shows how to write a single URL (http://localhost:5013/graphiql) to a text file (urls.txt) and pass it on to EyeWitness as its custom URL list.

```
# echo 'http://localhost:5013/graphiql' > urls.txt
# eyewitness --web -f urls.txt -d dvga-report

Starting Web Requests (1 Hosts)
Attempting to screenshot http://localhost:5013/graphiql
Finished in 8 seconds

[*] Done! Report written in the dvga-report folder!
```

Listing 4-15: Scanning multiple URLs with EyeWitness