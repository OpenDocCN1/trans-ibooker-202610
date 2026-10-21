Next, verify that the container is running by using the following command:

```
# sudo docker container ps
```

```
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
7b33cca84fc1 dvga "python3 app.py" About a minute ago
Up 0.0.0.0:5013->5013/tcp, :::5013->5013/tcp dvga
```

At this point, the target application should be up and running. Verify this by opening a web browser and entering http://localhost:5013 in the address bar. You should be able to access the application shown in Figure 2-6.

![](bbox=[0.22, 0.27, 0.88, 0.55])

Figure 2-6: The Damn Vulnerable GraphQL Application

As you can see, DVGA resembles Pastebin (https://pastebin.com), a web application that allows clients to submit random text snippets (such as source code or other text) and share them with others. These text snippets are also called pastes, a term we will use throughout this book as we walk through penetration-testing scenarios using DVGA. Pastes can have metadata, such as titles, content, author information, and so on. You will be able to see this information when we run queries against DVGA. Figure 2-7 shows an example of a paste in DVGA.

![](bbox=[0.22, 0.78, 0.64, 0.92])

Figure 2-7: A sample paste in DVGA