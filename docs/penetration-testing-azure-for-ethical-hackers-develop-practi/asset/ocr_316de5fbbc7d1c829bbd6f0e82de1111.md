# Finding Azure credentials

Outside of finding vulnerabilities in an Azure application, or service, the most common option for gaining access to an Azure subscription is through guessed, stolen, or "found" credentials.

# Guessing Azure AD credentials

From practical testing experience, the most common way to get into an Azure tenant is through weak or default credentials. While we have made massive technological advances over the years, users still like to use simple passwords, and administrators sometimes forget to implement identity security best practices.

There are three steps to a successful password guessing attack:

+   1. Obtain a username list.
+   2. Obtain a password list.
+   3. Decide and execute the guessing strategy.

Let's take a look.

# Obtaining a username list

Many organizations have a formal naming convention for usernames/email addresses. Some examples include `<firstname>.<lastname>@company.com` and `<firstname>.<lastname_initial>@company.com`. You could identify a company’s username format from user email addresses that are in the public domain (company websites, support channels, online employee profiles, GitHub, and so on).

You could then combine your knowledge of this naming convention with information about company users, found on sites such as LinkedIn, to construct usernames for a list of employees. You can also look at past breaches related to the organization to obtain this information.