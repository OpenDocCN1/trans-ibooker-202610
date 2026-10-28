# Interacting with the filesystem

The main process interacts with the filesystem using the fs Node.js library, which is built into the Electron framework. Let's see how we can use it:

+   1. Open the main.ts file that exists in the src\electron folder and import the following artifacts:

```
import { app, BrowserWindow, ipcMain } from 'electron';
import * as fs from 'fs';
import * as path from 'path';
```

The fs library is responsible for interacting with the filesystem. The path library provides utilities for working with file and folder paths. The ipcMain object allows us to work with the main process of Electron.

+   2. Create a variable that holds the path of the file containing the content of the editor:

```
const contentFile = path.join(app.getPath('userData'), 'content.html');
```

The file that keeps the content of the editor is the content.html file that exists inside the reserved userData folder. The userData folder is an alias for a special purpose system folder, different for each OS, and it is used to store application-specific files such as configuration. You can find more details about the userData folder as well as other system folders at https://www.electronjs.org/docs/api/app#appgetpathname.

> Important Note
> The getPath method of the app object works cross-platform and is used to get the path of special folders such as the home directory of a user or the application data.

+   3. Call the handle method of the ipcMain object to start listening for requests in the getContent channel:

```
ipcMain.handle('getContent', () => {
    if (fs.existsSync(contentFile)) {
        const result = fs.readFileSync(contentFile);
        return result.toString();
    }
    return '';
});
```