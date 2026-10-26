+   - **LDAP Injection Attack:** LDAP filters request directory service information—for example, if you use `(cn=heather, ou=managers, dc=packtpub, dc=com)`. This would be a search on the packtpub domain for a user called Heather who is a manager.

An attacker can create an LDAP filter to query the directory service such as Active Directory for information.

# Example 1: Authentication Bypass

One of the common LDAP injection attacks is an authentication bypass in which the attacker can use an LDAP filter, as follows:

```
(&(user=ID*)(userID*))(|(userID=*)(password=anything))
```

This will allow the attacker to gain access without a valid username or password.

# Example 2: Check for a User in a Department

The attacker will try and use an LDAP filter, such as the following example, in which they attempt to determine if there is a user called Ian who works in sales:

```
(&(userID=Ian Neil)department=sales(objectclass=user))
```

The best way to prevent an LDAP injection attack is to use a Block-List Input Validation, preventing the use of the following characters:

| ( | ) | & | * |
|---|---|---|---|
| < | > | + | ; |
| \\ | / | (blank) | “ or = |

If these characters appear as an input, we will "escape" them. This means that it will be interpreted as data and not code.

It should be mandatory for all companies to use LDAPS that will encrypt your directory service traffic. More information on LDAPS can be found in *Chapter 6* under *Secure Protocols and their Use Cases*. We should also use least privilege.

+   - **PowerShell:** PowerShell can perform tasks in a Windows environment. Each command is known as a cmdlet and can be saved to a script with a `.ps1` extension. PowerShell comprises a noun and a verb; for example, `Get-Help` will show you the help commands.