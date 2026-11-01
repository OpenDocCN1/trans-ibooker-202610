Finally, we will add some code to the VanillaPlaceholderContent class, as shown in Listing 1-7.

# Listing 1-7. Adding Component Logic to vanilla-placeholder-component

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Demo - vanilla-placeholder</title>
</head>
<body>
<script>
class VanillaPlaceholderContent extends HTMLElement {
    constructor() {
        super();
        const placeholder = document.createElement('template');
        const height = this.getAttribute('height') || '100px';
        const width = this.getAttribute('width') || '100px';
        placeholder.innerHTML = VanillaPlaceholderContent.template
        (height, width);
        this.appendChild(document.importNode(placeholder.content, true));
    }
    static template (height, width) {
        return `
<style>
.placeholder {
    background-color: red;
    width: ${height};
    height: ${width};
}
</style>
<div class='placeholder'>Placeholder</div>`;
    }
}
```