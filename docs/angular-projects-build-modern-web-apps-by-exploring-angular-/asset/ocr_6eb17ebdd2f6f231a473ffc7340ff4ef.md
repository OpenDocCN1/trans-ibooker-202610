# Project overview

In this project, we will build a desktop WYSIWYG editor that keeps its content local to the filesystem. Initially, we will build it as an Angular application using **ngx-wig**, a popular WYSIWYG Angular library. We will then convert it to a desktop application using Electron and learn how to sync content between Angular and Electron. We will also see how to persist the content of the editor into the filesystem. Finally, we will package our application as a single executable file that can be run in a desktop environment.

Build time: 1 hour.

# Getting started

The following software tools are required to complete this project:

+   - Angular CLI: A command-line interface for Angular that you can find at https://angular.io/cli.
+   - Visual Studio Code: A code editor that you can download from https://code.visualstudio.com/Download.
+   - GitHub material: The code for this chapter can be found in the Chapter05 folder at https://github.com/PacktPublishing/Angular-Projects-Second-Edition.

# Adding a WYSIWYG editor library for Angular

We will kick off our project by creating a WYSIWYG editor as a standalone Angular application first. Use the Angular CLI to create a new Angular application from scratch:

```
ng new my-editor --defaults
```

We pass the following options to the ng new command:

+   - `my-editor`: Defines the name of the application
+   - `--defaults`: Defines CSS as the preferred stylesheet format of the application and disables routing because our application will consist of a single component that will host the editor