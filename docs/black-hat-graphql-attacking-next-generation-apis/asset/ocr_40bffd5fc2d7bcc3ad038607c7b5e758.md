```
content
title
}
}
}
```

The mutation creates a new paste with the title Black Hat GraphQL and content I just spoofed the operation name. At the same time, we return the newly created paste’s content and title fields, which should have identical values.

Refresh the Audit page. You should be able to see that it now shows our spoofed operation name SpoofedOperationName under the GraphQL Operation column, as shown in Figure 8-13. This is what a security analyst might see if attempting to monitor GraphQL queries using operation names.

![](bbox=[0.22, 0.36, 0.87, 0.51])

Figure 8-13: The Audit page showing the modified operation name in DVGA

As we mentioned earlier, different GraphQL server implementations may allow operation names to include special characters, which could be an injection vector, so always test these whenever possible.

# Finding Stored XSS in the CreatePaste Mutation

When we create a new paste in DVGA, the GraphQL fields used in our createPaste mutation, such as title and content, are shown on the Public Pastes page. The screenshot in Figure 8-14 shows what this looks like.

![](bbox=[0.22, 0.78, 0.87, 0.92])

Figure 8-14: Paste structure and contents in DVGA