# # Getting started

The following software tools are required for completing this project:

+   * **Angular CLI**: A command-line interface for Angular that you can find at https://angular.io/cli
+   * **GitHub material**: The related code for this chapter that you can find in the Chapter03 folder at https://github.com/PacktPublishing/Angular-Projects-Second-Edition.

# ## Installing Clarity to an Angular application

Let's start creating our issue tracking system by scaffolding a new Angular application:

```
```
ng new issue-tracker --defaults
```
```

We use the ng new command of the Angular CLI to create a new Angular application with the following characteristics:

+   * issue-tracker: The name of the Angular application.
+   * --defaults: This disables Angular routing for the application and sets the stylesheet format to be CSS.

We now need to install the Clarity library in our Angular application. The VMware team has created an Angular CLI schematic that we can use. Navigate to the issue-tracker folder that was created with the preceding command and run the following add command of the Angular CLI:

```
```
ng add @clr/angular
```
```

The previous command will perform the following modifications in our Angular CLI workspace:

+   1. It will add all the necessary npm packages of the Clarity library to the dependencies section of the package.json file.
+   2. It will add the necessary styles in the configuration file of the Angular CLI workspace, angular.json:

```
```json
"styles": [
  "node_modules/@clr/ui/clr-ui.min.css",
  "src/styles.css"
]
```
```