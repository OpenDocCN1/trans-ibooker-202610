If the email or phone number are correctly verified, and it's not possible to spoof them, there is still the chance that the reset link is not correctly implemented. Sometimes these links include a parameter indicating the ID, such as the number or name of the user whose password is going to be reset. In this case, all that you need to do is to generate a link using a user that you control and change that parameter to one of the user whose password you want to reset.

Another possible fail is that such a reset link is not invalidated after the first, legitimate use. In this case, if an attacker gains access to such a link, by any means, they can access it again and reset the user's password.

# Vulnerabilities in 2FA implementations

The most common form of MFA in web applications is the use of a randomly generated number (four to eight digits) used as OTP that the user gets from a special device, a mobile app (such as Google Authenticator, Authy, 1Password, or LastPass Authenticator), or through an SMS or email sent by the server on request.

You can detect and take advantage of some implementation flaws in this process during a penetration test when the following conditions exist:

+   - OTP numbers are not completely random and can be predicted.
+   - OTPs are not linked to the user to whom they are assigned. This means that you can generate an OTP for one user and use it with another.
+   - The same password or token can be used multiple times.
+   - There is no limit for OTP submission attempts. This opens up the possibility of brute force attacks, which are more likely to be successful as OTPs are normally short strings of numbers.
+   - User information is not validated when sending the OTP by email or SMS, allowing an attacker to spoof the email address or phone number.
+   - The expiration time of the OTP is too long for the purposes of the application. This expands the time window for an attacker to get a valid, unused token.