
The preceding list is not exhaustive, and some of the extensions are already included in the **Angular Essentials** pack. However, you can browse more Angular extensions for VSCode at `https://marketplace.visualstudio.com/search?term=angular&target=VSCode`.

## Nx Console

The Nx Console is a VSCode extension developed by the Nrwl team that provides a graphical user interface over the Angular CLI. It contains most of the Angular CLI commands, and it uses the Angular CLI internally to execute each one. We will learn more about this extension in the *Building our application with Nx Console* section.

## Angular Language Service

The Angular Language Service extension provides various enhancements while editing HTML templates in an Angular application, including the following:

- Code autocompletion
- Compile error messages
- Go-to definition techniques

Code autocompletion is a feature that helps us find the right property or method to use while typing. It works by displaying a list of suggestions while we start typing in HTML content:

![](img/efea53d01337e7b223389b38dcdb0160_24_0.png)

Figure 1.3 – Code completion

In the previous screenshot, when we start typing the word `ti`, the Angular Language Service suggests the `title` component property. Notice that code completion only works for the public properties and methods in a component.

One of the most common issues when developing web applications is detecting errors before the application reaches production. This problem can be solved partially by the Angular compiler, which is bootstrapped upon building an Angular application for production. Moreover, the Angular Language Service can take this further by displaying compilation error messages far before our application reaches the compilation process:

![](img/efea53d01337e7b223389b38dcdb0160_25_0.png)

For example, if we accidentally misspell the name of a property or method of the component, the Angular Language Service will display an appropriate error message.

## Angular Snippets

The Angular Snippets extension contains a collection of Angular code snippets for TypeScript and HTML. In TypeScript, we can use it to create components, modules, or services in a blank TypeScript file:

![](img/efea53d01337e7b223389b38dcdb0160_25_1.png)

In an HTML template, we can use the extension to create useful Angular artifacts, such as the *ngFor directive, to loop through a list in HTML:

![](img/efea53d01337e7b223389b38dcdb0160_26_0.png)

Figure 1.6 – *ngFor snippet

Due to the widespread popularity and capabilities of the Angular CLI, it looks more convenient to use it for generating Angular artifacts in TypeScript. However, Angular Snippets does a great job with the HTML part, where there are more things to remember off by heart.

## Angular Evergreen

One of the primary factors that makes the Angular framework so stable is that it follows a regular release cycle based on semantic versioning. If we want our Angular applications to be packed with the latest features and fixes, we must update them regularly. But how can we stay up to date in the most efficient way? We can use the Angular Evergreen extension for that!

It compares the Angular and Angular CLI versions of an Angular CLI project with the latest ones and alerts you about whether you need to update it:

![](img/efea53d01337e7b223389b38dcdb0160_27_0.png)

It provides an easy-to-use user interface for executing the following commands:

- Upgrading Angular dependencies to the *latest* version
- Upgrading Angular dependencies to the *next* version
- Upgrading all npm dependencies

Angular Evergreen is the perfect extension for never staying out of date with your Angular projects.

## Material Icon Theme

The last extension in the list does not add much value regarding the productivity of the developer. Instead, it focuses on the discoverability and aesthetic point of view by modifying the icon theme of VSCode.

- **VSCode**: A code editor that you can download from https://code.visualstudio.com/Download.
- **Angular CLI**: The command-line interface for Angular that we introduced in the *Essential background theory and context* section.
- **GitHub material**: The related code for this chapter can be found in the Chapter01 folder at https://github.com/PacktPublishing/Angular-Projects-Second-Edition.

## Creating our first Angular application

To create a fresh new Angular application, we must execute the `ng new` command of the Angular CLI, passing the name of the application as an option:

```
ng new my-angular
```

The new command is used to create a new Angular application or a new Angular workspace. An Angular workspace is an Angular CLI project that contains one or more Angular applications, where some of them can be Angular libraries. So, when we execute the `ng new` command, we create an Angular workspace with an Angular application by default.

In the previous command, the name of our Angular application is `my-angular`. Upon executing the command, the Angular CLI will ask some questions to collect as much information as possible regarding the nature of the application that we want to create. Let's take a look:

1. Initially, it will ask whether we want to enable routing in our Angular application:

```
? Would you like to add Angular routing? (y/N)
```

Figure 1.9 – Angular routing

Routing in Angular is all about navigating between the components of an Angular application using a URL. In this project, we are not concerned with routing, so press *Enter* to accept the default value.

2. Then, the Angular CLI prompts us to select the style format that we want to use throughout the Angular application:

```
? Which stylesheet format would you like to use? (Use arrow keys)
> CSS
  SCSS   [ https://sass-lang.com/documentation/syntax#scss                ]
  Sass   [ https://sass-lang.com/documentation/syntax#the-indented-syntax ]
  Less   [ http://lesscss.org                                             ]
```

Figure 1.10 – Stylesheet format

3. Select a format from the list of available stylesheets and press *Enter*.

> **Tip**
> 
> You can use the arrow keys of your keyboard to navigate between the available stylesheet options.

The Angular CLI initiates the creation process of your Angular application, which consists of the following:

- Scaffolding the necessary folder structure of a typical Angular CLI project
- Installing the required npm dependencies and Angular packages
- Initializing Git in the Angular CLI project

This process may take some time, depending on the speed of your network. Once it has finished, you should have a new folder named `my-angular` in the path where you ran the `ng new` Angular CLI command.

Now, the time has finally come to run our Angular application and see it in action:

1. Open a Terminal window and navigate to the my-angular folder.
2. Run the following Angular CLI command:

```
ng serve
```

This will build the Angular application and start a built-in web server that we can use to preview it. The web server is started in watch mode; that is, it automatically rebuilds the Angular application whenever we make a change in the code. The first time an Angular application is built, it takes a considerable amount of time to complete, so we must be patient. You will know when the process has finished with no errors when you see the following message in the Terminal window:

```
** Angular Live Development Server is listening on localhost:4200, open your browser on http://localhost:4200/ **

✔ Compiled successfully.
```

Figure 1.11 – Angular build output

3. Fire up your favorite browser and navigate to `http://localhost:4200` to get a preview of your brand-new Angular application:

![](img/efea53d01337e7b223389b38dcdb0160_31_0.png)

Figure 1.12 – Minimal Angular application

The Angular CLI creates a minimal Angular application by default to provide us with a starting point for our Angular project. It contains some ready-made CSS styles and HTML content, which we will learn how to change according to our specifications in the following section.
