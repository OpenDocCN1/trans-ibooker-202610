To make it easier to share these values, we can create a new file, math-constants.js, and, using 'export', tell JavaScript that we are available to import that value. This is shown in Listing 5-2.

# Listing 5-2. Exporting Values in file math-constants.js

```
export const pi = 3.1415;
export const euler = 2.7182;
```

Now we can use these values in other files, using the type="module" in HTML files, as in Listing 5-3, or import in JS files, as in Listing 5-4.

# Listing 5-3. Using ES Modules in HTML

```
<script type="module" src="./math-constants.js"></script>
```

# Listing 5-4. Using ES Modules in JS Scripts

```
import {pi, euler} from "./math-constants.js";
```

Figure 5-1 offers a graphical view of ES Modules.

![](bbox=[0.2, 0.58, 0.76, 0.82])

# Figure 5-1. Graphical representation of ES Modules