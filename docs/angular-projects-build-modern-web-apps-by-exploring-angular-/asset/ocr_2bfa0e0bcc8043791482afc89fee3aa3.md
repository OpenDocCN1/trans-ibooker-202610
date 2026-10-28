The preceding file contains the TypeScript class of the component that will be generated. The selector and the templateUrl properties of the @Component decorator are built using the dasherize method and the name of the component. The name of the class contains a different method called classify that takes the name of the component as a parameter and converts it to title case.

+   4. Open the index.ts file of the tailwind-container folder, set the type of options to Schema and remove the return statement:

```
import { Rule, SchematicContext, Tree } from '@angular-devkit/schematics';
import { Schema } from './schema';

// You don't have to export the function as default.
// You can also have more than one rule factory
// per file.
export function tailwindContainer(_options: Schema): Rule {
  return (_tree: Tree, _context: SchematicContext) => {};
}
```

+   5. Add the following import statements at the top of the file:

```
import { normalize, strings } from '@angular-devkit/core';
import { apply, applyTemplates, chain, mergeWith, move, Rule, SchematicContext, Tree, url } from '@angular-devkit/schematics';
import { Schema } from './schema';
```

+   6. Insert the following code inside the tailwindContainer function:

```
_options.path = _options.path ?? normalize('src/app/' + _options.name as string);

  const templateSource = apply(url('./files'), [
    applyTemplates({
      classify: strings.classify,
      dasherize: strings.dasherize,
      name: _options.name
```