```
Superuser created successfully.
(ll_env)learning_log$
```

When you issue the command `createsuperuser`, Django prompts you to enter a username for the superuser ①. Here I’m using `ll_admin`, but you can enter any username you want. You can enter an email address or just leave this field blank ②. You’ll need to enter your password twice ③.

> **NOTE**
> 
> *Some sensitive information can be hidden from a site’s administrators. For example, Django doesn’t store the password you enter; instead, it stores a string derived from the password, called a hash. Each time you enter your password, Django hashes your entry and compares it to the stored hash. If the two hashes match, you’re authenticated. By requiring hashes to match, Django ensures that if an attacker gains access to a site’s database, they’ll be able to read the stored hashes but not the passwords. When a site is set up properly, it’s almost impossible to get the original passwords from the hashes.*

# Registering a Model with the Admin Site

Django includes some models in the admin site automatically, such as `User` and `Group`, but the models we create need to be added manually.

When we started the `learning_logs` app, Django created an *admin.py* file in the same directory as *models.py*. Open the *admin.py* file:

```
admin.py

from django.contrib import admin
```