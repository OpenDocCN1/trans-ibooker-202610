# GUIDELINE 21: USE COMMANDS TO ISOLATE WHAT THINGS ARE DONE

+   - Apply the *Command* design pattern with the intent to abstract and encapsulate an (possibly undoable) action;
+   - Be aware that the line between the *Command* and the *Strategy* design pattern is fluent;
+   - Use *Command* for both dynamic as well as static applications.

# Guideline 22: Prefer Value Semantics to Reference Semantics

In “Guideline 19: Use Strategy to Isolate How Things are Done” and “Guideline 21: Use Commands to Isolate What Things are Done” I have introduced you to the *Strategy* and *Command* design pattern, respectively. In both cases the examples were firmly built on the classic GoF style: they were using dynamic polymorphism by means of an inheritance hierarchy. With that classic object-oriented style, lacking a modern touch, I imagine that by now, all your nail-biting has gotten you in trouble with your manicurist. And you might be wondering: “Isn’t there another, a better way to implement *Strategy* and *Command*? A more “modern” approach?” Yes, rest assured, there is. And this approach is so important for the philosophy of what we commonly call “Modern C++” that it definitely justifies a separate section to explain the advantages. I am pretty sure your manicurist will understand the reason for this little detour.

# The Shortcomings of the GoF Style: Reference Semantics

The design patterns collected by the Gang of Four (GoF) and presented in the their book were introduced as object-oriented (OO) design patterns.