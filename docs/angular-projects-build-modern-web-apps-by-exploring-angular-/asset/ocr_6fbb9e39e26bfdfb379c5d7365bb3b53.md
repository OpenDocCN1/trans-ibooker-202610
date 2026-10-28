We configured our application to use the SCSS stylesheet format. Instead of creating the styles of our application manually, we will use the **Bootstrap CSS** library:

+   1. Execute the following command in a terminal window to install Bootstrap:

```
```
npm install bootstrap
```
```

We use the npm executable to install the bootstrap package from the npm registry in the previous command.

+   2. Add the following import statement at the top of the `styles.scss` file that exists in the `src` folder of our Angular application:

```
```
@import "~bootstrap/scss/bootstrap";
```
```

The `styles.scss` file contains CSS styles that are applied globally in our application. In the previous snippet, we import all the styles from the Bootstrap library into our application. The `@import` CSS rule accepts the absolute path of the `bootstrap.scss` file as an option, without adding the `.scss` extension. The `~` character represents the `node_modules` folder of our Angular application.

> **Important Note**
> The `node_modules` folder contains all the npm packages and libraries that our application needs, either during development or runtime.

In the following section, we will learn how to create the basic layout of our blog by creating components, such as the header and the footer.

# # Creating the basic layout of our blog

A blog typically has a header containing all the primary website links and a footer containing copyright information and other useful links. In the world of Angular, both can be represented as separate components.

The header component is used only once since it is added when our application starts up, and it is always rendered as the main menu of the website. In Angular, we typically create a module, named **core** by convention, to keep such components or services central to our application. To create the module, we use the `generate` command of the Angular CLI:

```
```
ng generate module core
```
```