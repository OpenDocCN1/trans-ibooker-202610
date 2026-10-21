# Information Disclosure

In this section, we will review publicly disclosed vulnerabilities that led exclusively to information disclosure issues. Some of the issues we covered earlier in this chapter also resulted in information disclosure outcomes, though these stemmed from other vulnerabilities such as broken access control mechanisms.

# Enumerating GraphQL Users (GitLab)

In 2021, Rapid7 identified CVE-2021-4191 in GitLab’s Community Edition and Enterprise Edition. The vulnerability allowed unauthenticated attackers to access user information in private GitLab instances that had specifically restricted their user registration interfaces through the users field.

For example, the following query returns information about users in GitLab instances, such as their name, username, and ID:

```
query {
  users {
    nodes {
      id
      name
      username
    }
  }
}
```

In addition to a user’s name and username, the vulnerability affected fields such as their email, location, user permissions, group memberships, state, and profile picture. Having access to so much information about users is useful for several reasons:

+   - Identifying accounts to attack. Knowledge of usernames and emails allowed threat actors to target specific accounts. Having access to user emails also allowed threat actors to pivot to other attacks, such as social engineering, by sending phishing emails to users.
+   - Identifying available groups. The vulnerability allowed attackers to infer information about the company running GitLab through their group memberships. Group memberships can reveal information such as acquisitions, subsidiaries, other company branches, regions where the company operates, and so on.
+   - Identifying individuals. The vulnerability allowed access to profile pictures, which could help threat actors target specific users on platforms outside of GitLab.
+   - Identifying state of accounts. Knowing the state of an account (whether it’s disabled or enabled) could make attacks such as brute forcing more effective; threat actors could target only accounts that are in an enabled state, allowing them to optimize their attacks.