![](bbox=[0.185, 0.135, 0.898, 0.465])

Figure 10.34 Styled Buttons and Container

# 10.7 Error handling

Under Name, Email, and Message controls is an error message. Currently it is unstyled and therefore not easily identifiable as such. Furthermore, we don’t want to show this error message until the user has interacted with the control. Nobody wants an error message yelling at them before they have even gotten started.

What we are going to do is style them to look like error messages, then we will hide them by default and only show them when appropriate. A bit of advanced warning, if you are coding along with this chapter and downloaded the starter files from Github you may have noticed a JavaScript file. This is where the code in that file is going to come into play. We will get more into details about that here shortly.

# 10.7.1 Styling error messages

Like most error messages on the web, we are going to make our text red. We will use the color we set in our --error custom property. We will also make the text bold and preface our error with the words "Error: " to clearly present it as such because as mentioned earlier in the chapter, we don’t want to use just color to convey meaning or intent. So that we can keep the wording consistent and not have to remember to include it everytime, instead of adding the verbiage at the beginning of each error, we will add it programmatically via CSS using the ::before pseudo element. We will also need to add a 2rem margin to the right of the error so that it can be aligned with the label and input text. Our rules to style our errors will therefore look as follows (Listing 10.25):