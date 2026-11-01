
Customized built-in elements inherit from another standard HTML element. For example, we can define one element that extends the <p> element, as in Listing 2-2.

### Listing 2-2. Defining a Customized CustomElement

```
class randomParagraphSizePlaceholder extends HTMLParagraphElement {
    constructor(){...}
}

customElements.define('random-paragraph-size-placeholder',
randomParagraphSizePlaceholder, {extends: 'p'});
```

Now we can use this element in our HTML document, as in Listing 2-3.

### Listing 2-3. Using a Customized CustomElement

```
<p is="random-paragraph-size-placeholder">Some text</p>
```

You can find a list of interfaces to inherit at https://html.spec.whatwg.org/multipage/indices.html#element-interfaces.

## Custom Elements’ Life Cycle Hooks

When we define a custom element, we can use life cycle hooks to run code in specific moments during the component life in our component. There are four main moments in our custom element that we can use.

- constructor: This is triggered when an instance of the element is created or upgraded. It is useful for initializing variables, add event listeners, or creating a Shadow DOM.
- connectedCallback: This is triggered each time a custom element is appended into a document. This will occur each time the node is moved and may happen before the element’s contents have been fully parsed.
- attributeChangedCallback (attrName, oldVal, newVal): This is invoked each time one of the custom element's attributes is added, removed, or changed. The observed attributes to notice a change are specified with the static get observedAttributes method.
- disconnectedCallback: This is invoked each time the custom element is disconnected from the document's DOM.

We can see all the preceding methods of the Web Components life cycle in Figure 2-1.

![](img/e276a43da8123ca0974f10d5c85948b9_35_0.png)

Figure 2-1. Web Components life cycle

## Building a Custom Element

To learn how to use the customElements object and the life cycle hooks, we are going to create the randomParagraphSizePlaceholder component. This simple component generates a random number between 12 and 50px and receives the attribute 'text'.

To use this component in our HTML document, we must call <random-paragraph-size-placeholder>, as in Listing 2-4.

### Listing 2-4. Using random-paragraph-size-placeholder

```
<random-paragraph-size-placeholder text="My Personal Text"></random-paragraph-size-placeholder>
```

Next, we must create a generic structure for an autonomous custom element, as in Listing 2-5.

### Listing 2-5. Declaring random-paragraph-size-placeholder Component

```
class RandomParagraphSizePlaceholder extends HTMLElement {
    constructor(){...}
}
customElements.define('random-paragraph-size-placeholder',
RandomParagraphSizePlaceholder);
```

With this, the web browser knows that we want to register a new HTML tag.
Later, we are going to add the life cycle hooks as methods, and we are going to add console.log(), to know when the method is triggered. (See Listing 2-6.)

### Listing 2-6. Defining random-paragraph-size-placeholder Component

```
class RandomParagraphSizePlaceholder extends HTMLElement {
    constructor(){
        console.log(`constructor.`)
    }
    connectedCallback() {
        console.log(`connectedCallback hook`);
    }
    disconnectedCallback() {
        console.log(`disconnectedCallback hook`);
    }
    attributeChangedCallback(attrName, oldVal, newVal) {
        console.log(`attributeChangedCallback hook`);
    }
}
customElements.define('random-paragraph-size-placeholder',
RandomParagraphSizePlaceholder);
```

To get the attributeChangedCallback() method to work correctly, we must add the static method observedAttributes() and return the properties that we want to observe. In this case, we only have the 'text' property, as in Listing 2-7.

### Listing 2-7. Adding Text to Be Observed in attributeChangedCallback

```
static get observedAttributes() {
    return ['text'];
}
```

Next, we’ll add the basic logic in the constructor, to generate random numbers and send them to the template method, as in Listing 2-8.

### Listing 2-8. Adding the Logic to Initialize random-paragraph-size-placeholder

```
constructor() {
    console.log(`constructor hook`);
    super();
    const placeholder = document.createElement('template');
    const myText = this.getAttribute('text') || 'Lorem Ipsum';
    const randomSize = Math.floor((Math.random() * (50 - 12 + 1)) + 12);
    placeholder.innerHTML = RandomParagraphSizePlaceholder.template
    (myText, randomSize);
    this.appendChild(document.importNode(placeholder.content, true));
}
```

Altogether, the code will look as shown in Listing 2-9.

### Listing 2-9. Final Code for random-paragraph-size-placeholder

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Demo - random-paragraph-size-placeholder</title>
</head>
<body>
<div id="parent">
    <random-paragraph-size-placeholder text="My Personal Text">
    </random-paragraph-size-placeholder>
</div>
<button id="myButton" onclick="removeElement()">Remove Element</button>
<script>
class RandomParagraphSizePlaceholder extends HTMLElement {
    constructor() {
        console.log(`constructor hook`);
        super();
        const placeholder = document.createElement('template');
        const myText = this.getAttribute('text') || 'Lorem Ipsum';
        const randomSize = Math.floor((Math.random() * (50 - 12 + 1)) + 12);
        placeholder.innerHTML = RandomParagraphSizePlaceholder.
        template(myText, randomSize);
        this.appendChild(document.importNode(placeholder.content, true));
    }
    static get observedAttributes() {
        return ['text'];
    }
    set text(val) {
        if (val) {
            this.setAttribute(`text`, val);
        } else {
            this.setAttribute(`text`, ``);
        }
    }
    get text() {
        return this.getAttribute('text');
    }
    connectedCallback() {
        console.log(`connectedCallback hook`);
    }
    disconnectedCallback() {
        console.log(`disconnectedCallback hook`);
    }
    attributeChangedCallback(attrName, oldVal, newVal) {
        console.log(`attributeChangedCallback hook`);
        console.log(`attrName`, attrName);
        console.log(`oldVal`, oldVal);
        console.log(`newVal`, newVal);
    }
    static template (myText, randomSize) {
        return `
    <div style="font-size:${randomSize}px">${myText}</div>`;
    }
}
customElements.define('random-paragraph-size-placeholder',
RandomParagraphSizePlaceholder);

const element = document.querySelector('random-paragraph-size-placeholder');

function removeElement() {
    const parentElement = document.getElementById('parent')
    parentElement.removeChild(element);
    const myButton = document.getElementById('myButton');
    myButton.disabled = true;
}
</script>
</body>
</html>
```

You'll also notice that I've added an extra function, removeElement, to see how disconnectedCallback() is triggered when I remove the component from the DOM. You can access the code for this book (https://github.com/carlosrojaso/apress-book-web-components) at git checkout chap-2.

## Summary

In this chapter, you learned the following:

- What the CustomElementRegistry object is and how to use it
- What the two main types of custom elements are
- What life cycle hooks are and when they are triggered

## CHAPTER 3

## HTML Template

In this chapter, we are going to examine HTML Template, another specification in the Web Components set. You are going to learn what an HTML Template is and how to use HTML templates in web components. We then will build a new web component for our collection.

### What Is an HTML Template?

The HTML Template specification defines the <template> element, to create fragments of markup to be unused in our custom element until we activate them later on runtime. These fragments can be cloned and inserted in HTML by script.

The content inside a <template> has the following properties:

- The content will not render until it is activated. The markup inside <template> is hidden and doesn’t render.
- The content will not have side effects. The scripts, images, and media tags don’t run until activated.
- The content will not be considered to be in the Document Object Model (DOM). Using getElementById() or querySelector() won’t return child nodes of a template.

The basic way to use <template> is indicated in Listing 3-1.

### Listing 3-1. Basic Example of Using <template>

```
<template id="my-error-message">
    <p>
        Some error messages.
    </p>
</template>
```
