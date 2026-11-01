# Listing 5-7. Using Modules in MathOperationsComponent

```
<script type="module">
import {pi, euler, ln2} from './math-constants.js';

class MathOperationsComponent extends HTMLElement {
    constructor() {
    }
}
customElements.define('math-operations-component',
MathOperationsComponent);
</script>
```

First in the preceding code snippet is type="module" in the script tag for modules in our code. Second is 'import', for constants that we declared in math-constants.js.

Now, in our component, we are going to create getCircumference, getCalcOneYear, and getLN2, to return the value that we require for the parameters that we sent in the call. This is shown in Listing 5-8.

# Listing 5-8. Using Modules in MathOperationsComponent

```
getCircumference(radius) {
    return 2 * pi * radius;
}

getCalcOneYear(interestRate, currentVal) {
    return currentVal * (euler ** interestRate);
}

getLN2() {
    return ln2;
}
```

Note that here we are using the constants that we imported from the module.

Finally, we add the logic in constructor(), to process the parameters that we send in the 'operation', and 'initialValue' attributes in the component, and create the methods for the template and styles that we want to reveal our information in the document, as in Listing 5-9.