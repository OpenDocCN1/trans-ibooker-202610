# Listing 2-4. Using random-paragraph-size-placeholder

```
<random-paragraph-size-placeholder text="My Personal Text"></random-paragraph-size-placeholder>
```

Next, we must create a generic structure for an autonomous custom element, as in Listing 2-5.

# Listing 2-5. Declaring random-paragraph-size-placeholder Component

```
class RandomParagraphSizePlaceholder extends HTMLElement {
    constructor(){...}
}
customElements.define('random-paragraph-size-placeholder',
RandomParagraphSizePlaceholder);
```

With this, the web browser knows that we want to register a new HTML tag.
Later, we are going to add the life cycle hooks as methods, and we are going to add console.log(), to know when the method is triggered. (See Listing 2-6.)

# Listing 2-6. Defining random-paragraph-size-placeholder Component

```
class RandomParagraphSizePlaceholder extends HTMLElement {
    constructor(){
        console.log(`contructor.`)
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