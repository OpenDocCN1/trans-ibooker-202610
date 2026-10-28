```
options: {
    configFile: path.join(src, 'tsconfig.json')
    }
}
]
},
  target: 'electron-main'
};
```

The preceding file configures webpack in our application using the following options:

+   - mode: Indicates that we are currently running in a development environment.
+   - devtool: Enables source map file generation for debugging purposes.
+   - entry: Indicates the main entry point of the Electron application, which is the main.ts file.
+   - output: Defines the path and the filename of the Electron bundle that will be generated from webpack. The path property points to the same folder that is used from the Angular CLI to generate the bundle of the Angular application. The filename property is set to shell.js because the default one generated from webpack is main.js, and it will cause a conflict with the main.js file generated from the Angular application.
+   - module: Instructs webpack to load the ts-loader plugin for handling TypeScript files.
+   - target: Indicates that we are currently running in the main process of Electron.

webpack now contains all information needed to build and bundle the Electron application. On the other hand, the Angular CLI takes care of building the Angular application. Let's see how we can combine them and run our desktop application:

+   1. Run the following npm command to install the concurrently npm package:

```
npm install -D concurrently
```

The concurrently library enables us to execute multiple processes concurrently. In our case, it will enable us to run the Angular and Electron applications in parallel.