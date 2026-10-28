
## To get the most out of this book

You will need a version of Angular 12 installed on your computer, preferably the latest minor one. All code examples have been tested using Angular 12.0.0 on Windows but they should work with any future release of Angular 12 as well.

| Software/hardware covered in the book | Operating system requirements |
|---|---|
| Angular 12 | Windows, macOS, or Linux |
| Visual Studio Code | Windows, macOS, or Linux |
| Scully | Windows, macOS, or Linux |
| Clarity Design | Windows, macOS, or Linux |
| Angular PWA | Windows, macOS, or Linux |
| Electron | Windows, macOS, or Linux |
| Ionic | Windows, macOS, or Linux |
| CesiumJS | Windows, macOS, or Linux |
| Firebase | Windows, macOS, or Linux |
| GitHub API | Windows, macOS, or Linux |
| Nx monorepo tools | Windows, macOS, or Linux |
| NgRx | Windows, macOS, or Linux |
| Angular Material | Windows, macOS, or Linux (Any) |
| Angular CDK | Windows, macOS, or Linux (Any) |
| Angular schematics | Windows, macOS, or Linux (Any) |

**If you are using the digital version of this book, we advise you to type the code yourself or access the code from the book's GitHub repository (a link is available in the next section). Doing so will help you avoid any potential errors related to the copying and pasting of code.**

## Download the example code files

You can download the example code files for this book from GitHub at `https://github.com/PacktPublishing/Angular-Projects-Second-Edition`. If there's an update to the code, it will be updated in the GitHub repository.

We also have other code bundles from our rich catalog of books and videos available at `https://github.com/PacktPublishing/`. Check them out!

## Download the color images

We also provide a PDF file that has color images of the screenshots and diagrams used in this book. You can download it here: `https://static.packt-cdn.com/downloads/9781800205260_ColorImages.pdf`.

## Conventions used

There are a number of text conventions used throughout this book.

Code in text: Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles. Here is an example: "It will add all the necessary npm packages of the Clarity library to the dependencies section of the package.json file."

A block of code is set as follows:

```html
<div class="main-container">
  <div class="content-container">
    <div class="content-area">
      <app-issue-list></app-issue-list>
    </div>
  </div>
</div>
```

When we wish to draw your attention to a particular part of a code block, the relevant lines or items are set in bold:

```json
"styles": [
  "node_modules/@clr/ui/clr-ui.min.css",
  "src/styles.css"
]
```

Any command-line input or output is written as follows:

```bash
ng add @angular/material --theme=indigo-pink --typography=true --animations=true
```

**Bold:** Indicates a new term, an important word, or words that you see onscreen. For instance, words in menus or dialog boxes appear in **bold**. Here is an example: "Click on the **ADD NEW ISSUE** button and enter the details of a new issue."

> **Tips or important notes**
> Appear like this.

## Get in touch

Feedback from our readers is always welcome.

**General feedback:** If you have questions about any aspect of this book, email us at `customercare@packtpub.com` and mention the book title in the subject of your message.

**Errata:** Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you have found a mistake in this book, we would be grateful if you would report this to us. Please visit `www.packtpub.com/support/errata` and fill in the form.

**Piracy:** If you come across any illegal copies of our works in any form on the internet, we would be grateful if you would provide us with the location address or website name. Please contact us at `copyright@packt.com` with a link to the material.

**If you are interested in becoming an author:** If there is a topic that you have expertise in and you are interested in either writing or contributing to a book, please visit `authors.packtpub.com`.

## Share Your Thoughts

Once you've read *Angular Projects Second Edition*, we'd love to hear your thoughts! Please click here to go straight to the Amazon review page for this book and share your feedback.

Your review is important to us and the tech community and will help us make sure we're delivering excellent quality content.

# 1
Creating Your First Web Application in Angular

**Angular** is a popular and modern **JavaScript** framework that can run on different platforms additional to the web, such as desktop and mobile. Angular applications are written in **TypeScript**, a superset of JavaScript that provides syntactic sugar such as strong typing and object-oriented techniques.

Angular applications are created and developed using a command-line tool made by the Angular team called the **Angular CLI**. It automates many development tasks, such as scaffolding, testing, and deploying an Angular application, which would take a lot of time to configure manually.

The popularity of the Angular framework is considerably reflected in its broad support of tooling. The **Visual Studio Code (VSCode)** editor contains various extensions that enhance the development experience when working with Angular.

In this chapter, we will cover the following topics:

- Introduction to Angular
- Introduction to the Angular CLI
- Exploring the rich ecosystem of Angular tooling in VSCode
- How to create our first Angular application
- How to use Nx Console for automating Angular CLI commands

## Essential background theory and context

The Angular framework is a cross-platform JavaScript framework that can run on a wide range of environments, including the web, servers, mobile, and desktop. It consists of a collection of JavaScript libraries that we can use for building highly performant and scalable web applications. The architecture of an Angular application is based on a hierarchical representation of components. Components are the fundamental building blocks of an Angular application. They represent and control a particular portion of a web page called the *view*. Some examples of components are as follows:

- A list of blog posts
- An issue reporting form
- A weather display widget

Components of an Angular application can be logically organized as a tree:

![](img/efea53d01337e7b223389b38dcdb0160_20_0.png)

An Angular application typically has one main component, called **AppComponent**, by convention. Each component in the tree can communicate and interact with its siblings using an application programming interface defined by each one.

An Angular application can have many features that are called modules. Each module serves a block of single functionality that corresponds to a particular application domain or workflow. Angular modules are used to group Angular components that share similar functionality:

![](img/efea53d01337e7b223389b38dcdb0160_21_0.png)

In the previous diagram, the dashed line circles represent Angular modules. An Angular application typically has one main module, called **AppModule**, by convention. Each module can import other modules in an Angular application if they wish to use part of their functionality.

The functionality of a module can be further analyzed in the presentational and business logic of a feature. Angular components should only be responsible for handling the presentational logic and delegating business logic tasks to services. The Angular framework provides Angular services to components using a built-in **dependency injection (DI)** mechanism.

The Angular DI framework uses special-purpose objects, called injectors, to hide much of the complexity of providing dependencies to an Angular application. Components are not required to know any of the actual implementation of an Angular service. They only need to ask for it from an injector.

An Angular service should follow the single responsibility principle, and it should not cross boundaries between different Angular modules. Some examples of services are as follows:

- Access data from a backend API using the HTTP protocol.
- Interact with the local storage of the browser.
- Error logging.
- Data transformations.

An Angular developer does not need to remember how to create components, modules, and services off by heart while building an Angular application. Luckily, the Angular CLI can assist us by providing a command-line interface to accomplish these tasks.

## Introduction to the Angular CLI

The Angular CLI is a tool created by the Angular team that improves the developer experience while building Angular applications. It hides much of the complexity of scaffolding and configuring an Angular application while allowing the developer to concentrate on what they do best – coding! Before we can start using the Angular CLI, we need to set up the following prerequisites in our system:

- **Node.js:** A JavaScript runtime that is built on the v8 engine of Chrome. You can download any **Long-Term Support (LTS)** version from https://nodejs.org/en.
- **npm:** A package manager for the Node.js runtime.

We can then install the Angular CLI using npm from the command line:

```bash
npm install -g @angular/cli
```

We can use the `-g` option to install the Angular CLI globally since we want to create Angular applications from any path of our operating system.

> **Important note**
> Installing the Angular CLI may require administrative privileges in some operating systems.
