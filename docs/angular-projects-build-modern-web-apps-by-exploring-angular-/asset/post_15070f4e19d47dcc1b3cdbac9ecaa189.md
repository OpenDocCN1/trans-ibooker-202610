
Each schematic in the collection contains a short description, as indicated by the `description` property, and a `factory` property that points to the main entry point of the schematic using a special syntax. It contains the filename `./my-schematics/index`, followed by the `#` character, and the name of the function exported by that file, named `mySchematics`.

The main entry point of a schematic contains a rule factory method that is exported by default and returns a `Rule` object:

### index.ts

```typescript
import { Rule, SchematicContext, Tree } from '@angular-devkit/schematics';

// You don't have to export the function as default.
// You can also have more than one rule factory
// per file.
export function mySchematics(options: any): Rule {
  return (tree: Tree, _context: SchematicContext) => {
    return tree;
  };
}
```

A schematic does not interact directly with the filesystem. Instead, it creates a virtual filesystem that is represented by a `Tree` object. The virtual filesystem contains a staging area where all transformations from schematics happen. This area aims to make sure that any transformations that are not valid will not propagate to the actual filesystem. As soon as the schematic is valid to execute, the virtual filesystem will apply the changes to the real one. All transformations of a schematic operate in a `SchematicContext` object.

In the following section, we will learn how to use the schematics CLI and create a component generation schematic.

## Creating a Tailwind component

Tailwind is a very popular CSS framework that enforces a utility-first core principle. It contains classes and styles that can be used in Angular applications to create easily composable user interfaces.

We will use the schematics API of the Angular CLI to build a generation schematic for Angular components. The schematic will generate a new Angular component that is styled with a Tailwind container layout.

> **Important note**
> 
> The schematic that we will build does not need to have Tailwind CSS installed by default. However, the application in which we will use the schematic does require it.

Let's see how we can accomplish that:

1. Execute the following schematics CLI command to add a new schematic to our collection:
   
   ```bash
   schematics blank tailwind-container
   ```
   
   The preceding command will update the `collection.json` file to contain a new entry for the `tailwind-container` schematic. It will also create a `tailwind-container` folder in the `src` folder of our workspace.

2. Create a `schema.json` file inside the `tailwind-container` folder and add the following content:

```json
{
  "$schema": "http://json-schema.org/schema",
  "id": "TailwindContainerSchema",
  "title": "My Tailwind Container Schema",
  "type": "object",
  "properties": {
    "name": {
      "description": "The name of the component.",
      "type": "string"
    },
    "path": {
      "type": "string",
      "format": "path",
      "description": "The path to create the component.",
      "visible": false
    }
  },
  "required": ["name"]
}
```

Each schematic can have a JSON schema file that defines the options that are available when running the schematic. Since we want to create a component generation schematic, we need a `name` and a `path` property for our component. Each of these properties has metadata associated with it, such as the type and the description. The name of the component is required when invoking the schematic as indicated by the `required` array property.

3. Open the `collection.json` file and set the properties of the `tailwind-container` schematic as follows:

```json
{
  "$schema": "../node_modules/@angular-devkit/schematics/collection-schema.json",
  "schematics": {
    "my-schematics": {
      "description": "A blank schematic.",
      "factory": "./my-schematics/index#mySchematics"
    },
    "tailwind-container": {
      "description": "Generate a Tailwind container component",
      "factory": "./tailwind-container/index#tailwindContainer",
      "schema": "./tailwind-container/schema.json"
    }
  }
}
```

In the preceding file, we set a proper description for our schematic. We also add the `schema` property that points to the absolute path of the `schema.json` file we created in step 3.

4. Create a `schema.ts` file inside the `tailwind-container` folder and add the following content:

```typescript
export interface Schema {
    name: string;
    path: string;
}
```

The preceding file defines the `Schema` interface that contains mapping properties to those defined in the `schema.json` file.

We have now created all the underlying infrastructure that we will use to create our schematic. Let's see how to write the actual code that will run our schematic:

1. Create a folder named `files` inside the `tailwind-container` folder.

2. Create a file called `__name@dasherize__.component.html.template` inside the `files` folder and add the following contents:

```html
<div class="container mx-auto">
</div>
```

The preceding file denotes the template of the component that our schematic will generate. The `__name` prefix will be replaced by the name of the component that we will pass as an option in the schematic. The `@dasherize__` syntax indicates that the name will be separated with dashes and converted to lowercase if passed in camel case.

3. Create a file called `__name@dasherize__.component.ts.template` and add the following contents:

```typescript
import { Component } from '@angular/core';

@Component({
    selector: 'my-<%= dasherize(name) %>',
    templateUrl: './<%= dasherize(name) %>.component.html'
})
export class My<%= classify(name) %>Component {}
```

The preceding file contains the TypeScript class of the component that will be generated. The `selector` and the `templateUrl` properties of the `@Component` decorator are built using the `dasherize` method and the `name` of the component. The name of the class contains a different method called `classify` that takes the name of the component as a parameter and converts it to title case.

4. Open the `index.ts` file of the `tailwind-container` folder, set the type of `options` to `Schema` and remove the `return` statement:

```typescript
import { Rule, SchematicContext, Tree } from '@angular-devkit/schematics';
import { Schema } from './schema';

// You don't have to export the function as default.
// You can also have more than one rule factory
// per file.
export function tailwindContainer(_options: Schema): Rule {
  return (_tree: Tree, _context: SchematicContext) => {};
}
```

5. Add the following import statements at the top of the file:

```typescript
import { normalize, strings } from '@angular-devkit/core';
import { apply, applyTemplates, chain, mergeWith, move, Rule, SchematicContext, Tree, url } from '@angular-devkit/schematics';
import { Schema } from './schema';
```

6. Insert the following code inside the `tailwindContainer` function:

```typescript
_options.path = _options.path ?? normalize('src/app/' + _options.name as string);

  const templateSource = apply(url('./files'), [
    applyTemplates({
      classify: strings.classify,
      dasherize: strings.dasherize,
      name: _options.name
    }),
    move(normalize(_options.path as string))
]);
```

In the preceding code, first, we set the `path` property of the component in case one is not passed in the schematic. By default, we create a folder inside the `src\app` folder that has the same name as the component. We then use the `apply` method to read the template files from the `files` folder and pass the `dasherize`, `classify`, and `name` properties using the `applyTemplates` function. Finally, we call the `move` method to create the generated component files in the provided path.

7. Add the following statement at the end of the factory function:

```typescript
return chain([
    mergeWith(templateSource)
]);
```

In the preceding snippet, we call the `chain` method to execute our schematic, passing the result of the `mergeWith` function that uses the `templateSource` variable we created in *step 6*.

Now we can go ahead and test our new component schematic:

1. Execute the following `npm` command to build the schematic:

```bash
npm run build
```

The preceding command will invoke the TypeScript compiler and transpile the TypeScript source files into JavaScript. It will generate the JavaScript output files into the same folders, side by side, with the TypeScript ones.

2. Run the following command to install the schematics library into our global npm cache:

```bash
npm link
```

The preceding command is used so that we can install the schematics later without querying the public npm registry.

3. Execute the following Angular CLI command in a folder of your choice outside the workspace to scaffold a new Angular application with default options:

```bash
ng new my-app --defaults
```
