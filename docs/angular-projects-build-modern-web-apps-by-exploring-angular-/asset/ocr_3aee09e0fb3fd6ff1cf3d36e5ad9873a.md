When the main process receives a request in this channel, it uses the existsSync method of the fs library to check whether the file with the content of the editor exists already. If it exists, it reads it using the readFileSync method and returns its content to the renderer process.

+   4. Call the handle method again, but this time for the setContent channel:

```
ipcMain.handle('setContent', ({}, content: string) => {
  fs.writeFileSync(contentFile, content);
});
```

In the preceding snippet, we use the writeFileSync method of the fs library to write the value of the content property in the file.

+   5. Open the package.json file and change the version of the @types/node npm package:

```
"@types/node": "^15.6.0"
```

Now that we have connected the Angular and the Electron application, it is time to preview our WYSIWYG desktop application:

+   1. Execute the start:desktop npm script and press F5 to run the application.

+   2. Use the editor and its toolbar to enter some content such as the following:

![](bbox=[0.13, 0.51, 0.86, 0.62])

Figure 5.5 – Editor content

+   3. Close the application window and re-run the application. If everything worked correctly, you should see the content that you had entered inside the editor.

Congratulations! You have enriched your WYSIWYG editor by adding persistence capabilities to it. In the following section, we will take the last step toward creating our desktop application, and we will learn how to package it and distribute it.