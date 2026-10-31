Fortunately for penetration testers, most applications use the basic pattern of HTML form, sent through a POST request including the username and password as parameters and getting a redirect to the user's home page on successful login, and an error or redirection to the login page if failed. You will now examine two methods used to execute a dictionary attack on this kind of form. The same principle applies to almost all form-based authentication, with some modifications on how the responses are interpreted and the required parameters for submission.

# Using Burp Suite Intruder

As in a basic authentication attack, you first need to identify the request that performs the actual authentication and its parameters in order to attack the correct ones.

In the following screenshot, on the left-hand side, you'll see OWASP Bricks in the authentication form (in the Vulnerable Virtual system main menu, go to Bricks | Login pages | Login #3), and on the right-hand side, you can see the request via the POST method. You'll observe that the username and passwd parameters are sent in the body, while there is no Authorization header:

![](bbox=[0.13, 0.46, 0.87, 0.7])