# 7
Cross-Site Request Forgery,
Identification, and Exploitation

**Cross-Site Request Forgery (CSRF)** is often mistakenly perceived as a vulnerability that is similar to XSS. XSS exploits the trust a user has in a particular site, which makes the user believe any information presented by the website. On the other hand, CSRF exploits the trust that a website has in a user's browser, which has the website execute any request coming from an authenticated session without verifying if the user wanted to perform that specific action.

In a CSRF attack, the attacker makes authenticated users perform unwanted actions in the web application in which they are authenticated. This is accomplished through an external site that the user visits, which triggers these actions.

CSRF can exploit every web application function that requires a single request within an authenticated session if sufficient defense is not implemented. Here are some examples of the actions that attackers can perform through a CSRF attack:

+   - Changing user details, such as email address and date of birth, in a web application
+   - Making fraudulent banking transactions
+   - Conducting fraudulent up-voting and down-voting on websites
+   - Adding items to a shopping cart on an e-commerce website or buying items without the user's knowledge
+   - Preconditions for a CSRF attack