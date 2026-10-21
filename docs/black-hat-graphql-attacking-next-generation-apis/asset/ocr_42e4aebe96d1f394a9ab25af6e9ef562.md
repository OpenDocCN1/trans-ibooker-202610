![](bbox=[0.23, 0.07, 0.9, 0.4])

Figure 2-3: The available Altair Desktop client versions

Next, we need to change the permissions on the downloaded file to be able to run it:

```
# chmod u+x altair_x86_64_linux.AppImage
```

Now we can execute the file. It should load the client, as shown in Figure 2-4.

```
# ./altair_x86_64_linux.AppImage
```

After you’ve set the right permissions, you should also be able to run the application directly by clicking the Altair Desktop icon located on your Kali Desktop.

Let’s now verify that the client is working as expected. Open it and, in the Enter URL address bar, enter http://lab.blackhatgraphql.com/graphql. This will ensure that any query we execute will be sent directly to this address. Now, in the left-side Query pane, remove the existing code comments (lines that start with the # symbol) and enter the following query instead:

```
query {
  users {
    username
  }
}
```