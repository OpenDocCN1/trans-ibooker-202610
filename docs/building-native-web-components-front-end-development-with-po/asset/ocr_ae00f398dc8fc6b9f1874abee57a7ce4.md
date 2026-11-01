attributeChangedCallback (attrName, oldVal, newVal): This is invoked each time one of the custom element's attributes is added, removed, or changed. The observed attributes to notice a change are specified with the static get observedAttributes method.

disconnectedCallback: This is invoked each time the custom element is disconnected from the document's DOM.

We can see all the preceding methods of the Web Components life cycle in Figure 2-1.

![](bbox=[0.26, 0.3, 0.75, 0.64])

Figure 2-1. Web Components life cycle

# Building a Custom Element

To learn how to use the customElements object and the life cycle hooks, we are going to create the randomParagraphSizePlaceholder component. This simple component generates a random number between 12 and 50px and receives the attribute 'text'.

To use this component in our HTML document, we must call <random-paragraph-size-placeholder>, as in Listing 2-4.