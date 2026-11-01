![](bbox=[0.305, 0.098, 0.73, 0.337])

# Figure 1-18. Placeholder component

The basic usage of this component in our HTML is as in Listing 1-3.

# Listing 1-3. Using vanilla-placeholder-component

```
<vanilla-placeholder-content></vanilla-placeholder-content>
```

We can add some attributes, as in Listing 1-4.

# Listing 1-4. Using vanilla-placeholder-component with Attributes

```
<vanilla-placeholder-content height="100px" width="50px">
</vanilla-placeholder-content>
```

Our component accepts height and width properties to customize the size, but if we don’t provide that info, we are going to assign 100px by default for both.

First we must create a file index.html and fill it with a basic structure, as shown in Listing 1-5.

# Listing 1-5. index.html—Basic Structure

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
```