In this initial example, we have called the `decrement` tag three times. Since the `decrement` variable starts with a negative value, after calling it three times, we will receive the following values:

-1
-2
-3

> **Important note:**
> Besides the two differences that we have made, `decrement` shares all the rules and limitations of the `increment` tag, meaning that the `decrement` tag works independently of the variables created using the `assign` and `capture` methods. We cannot call it independently without initializing the `decrement` tag.

Variables are a potent tool that, combined with the iteration tags, will bring us one step closer to writing more concise and reusable code.

# Iterations tags

**Iterations tags** are different Liquid programming logic types that allow us to run blocks of code repeatedly. Using iteration tags will save us the time that it would otherwise take us to execute code for each occurrence manually; plus, it will make our code much more concise and readable. To keep the topic concise, we will only mention some of the most used iteration tags and their parameters, which is more important than listing them all as they are all created using similar concepts. We can divide the iteration tags into four separate groups:

+   - `for/else`
+   - `jump statements`
+   - `for parameters`
+   - `cycle`

# The for/else tags

In the previous chapter, we had the chance to use the `for` loop in one of our examples when we explained the arrays inside the *Understand the types of data* topic. However, we have not had the chance to explain all the possibilities that the `for` loop gives us. The `for` loop is a type of Liquid programming logic that allows us to loop over any code block or array and output the result of that loop for further use.