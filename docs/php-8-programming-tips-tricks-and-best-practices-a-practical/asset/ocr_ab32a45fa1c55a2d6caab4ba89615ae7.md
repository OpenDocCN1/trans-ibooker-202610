The functionality represented by the new interface doesn't add anything new to the existing DOM capabilities. Its main purpose is to bring the PHP DOM extension in line with the living standard.

> **Tip**
> There is another purpose for architecturally renovating the DOM extension in the future. In future versions of PHP, the DOM extension will have the capability of manipulating an entire branch of the DOM tree. When you issue an `append()`, for example, in the future, you will be able to append not only the one node, but all its child nodes as well. For more information, see the following RFC: https://wiki.php.net/rfc/dom_living_standard_api.

Now, let's have a look at the second new interface.

# Examining the new DOMChildNode interface

The second of the two new interfaces is **DOMChildNode**. The `DOMElement` and `DOMCharacterData` classes implement this interface in PHP 8.

Here is the interface definition:

```
interface DOMChildNode {
    public readonly ?DOMElement $previousElementSibling;
    public readonly ?DOMElement $nextElementSibling;
    public function remove() : void;
    public function before(
        ...DOMNode|string|null $nodes) : void;
    public function after(
        ...DOMNode|string|null $nodes) : void;
    public function replaceWith(
        ...DOMNode|string|null $nodes) : void;
}
```