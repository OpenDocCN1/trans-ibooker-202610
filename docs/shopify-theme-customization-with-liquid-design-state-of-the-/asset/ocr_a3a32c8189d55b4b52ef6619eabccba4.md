2. As we can see, the collection form that we have created already contains the two necessary things we need: the submit button and the variant id that we have stored inside a hidden input. For more straightforward navigation, let's start by assigning a new class called `collection-submit` to the submit button:

```
```liquid
{% form "product", product %}
  <input type="hidden" name="id" value="{{
    product.first_available_variant.id }}" />
  <input type="submit" class="collection-submit"
    value="Add to Cart"/>
{% endform %}
```
```

3. With the proper selector in place, we can now use an `addEventListener` on the submit button to capture the click event and pass the object to the function that we will create next:

```
```javascript
const addSelector = document.querySelectorAll
  (".collection-submit");
if (addSelector.length) {
  for (let i = 0; i < addSelector.length; i++) {
    addSelector[i].addEventListener('click', function(e) {
      e.preventDefault();
      addCart(this);
    });
  }
}
```
```

4. In the preceding example, we created an `addSelector` constant for capturing the click event. Using `preventDefault()`, we canceled any current events' flows and passed the object of the clicked element to the `addCart` function. Now, let's look at creating the `addCart` function:

```
```javascript
const addCart = (el) => {
  let formData = {
    'items': [
      {
        id: el.previousElementSibling.value
      }
    ]
```
```