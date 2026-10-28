```
plugins: [
    new CopyWebpackPlugin({
        patterns: [
            {
                context: path.join(process.cwd(), 'src',
                'electron'),
                from: 'package.json'
            }
        ]
    })
]
```

We use the CopyWebpackPlugin to copy the package.json file from the src\electron folder into the dist\my-editor folder while building the application in production mode.

5. Run the following command to build the application in production mode:

```
npm run build:electron
```

6. Now run the following npm command to package it:

```
npm run package
```

The preceding command will package the application for the OS that you are currently running on, which is the default behavior of the electron-packager library. You can alter this behavior by passing additional options, which you will find in the GitHub repository of the library listed in the Further reading section.

7. Navigate to the dist folder of the Angular CLI workspace. You will find a folder called my-editor-{OS}, where {OS} is your current OS and its architecture. For example, in Windows, it will be my-editor-win32-x64. Open the folder, and you will get the following files: