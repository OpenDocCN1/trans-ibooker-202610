# 10
Styling forms

# This chapter covers

+   - Styling input fields
+   - Styling radio buttons and checkboxes
+   - Styling drop down selects
+   - Accessibility considerations
+   - :focus versus :focus-visible
+   - :where and :is pseudo classes
+   - accent-color property

Forms are everywhere in our applications. Whether contact forms, login screens, or core to the application’s functionality, they are truly omnipresent. The design of the form however can easily make or break the user’s experience. In this chapter we will style a form and look at some of the accessibility considerations we need to make sure to address. We will look at some of the challenges that come with styling some radio and checkbox inputs and drop downs as well as cover some of our options for styling error messaging.

A form in this context is a section of code, contained inside of a HTML <form> element containing controls (form fields) that the user interacts with in order to submit data to our website or application.

Because contact forms are so prevalent across applications and websites, we will use a contact form as the basis for our project. Our form contains 2 input fields, a dropdown, radio buttons, a checkbox, and a text area. We also have a header at the top and 2 buttons, Send and Reset, at the end of the form. Figure 10.1 shows our starting point, the raw HTML without any styles applied and what we aim to accomplish.