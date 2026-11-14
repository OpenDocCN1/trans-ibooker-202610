
You provide an LDAP filter (in this case, searching for groups with the name of `Management`), and then call the `FindOne()` method. The `FindOne()` method returns the first search result that matches the filter, so we retrieve its actual Active Directory entry. If you expect your query to return multiple results, use the `FindAll()` method instead. Although the Solution searches on the group’s name, you can search on any field in Active Directory—the `mailNickname` and `sAMAccountName` are two other good choices.

When you do this search, always try to restrict it to the lowest level of the domain possible. If we know that the `Management` group is in the `Sales` OU, it would be better to bind to that OU instead:

```
$domain = [adsi] "LDAP://localhost:389/ou=Sales,dc=Fabrikam,dc=COM"
```

For more information about the LDAP search filter syntax, search [the Microsoft documentation site](http://msdn.microsoft.com) for “LDAP Search Filter Syntax.”

The cmdlet to search for a security or distribution group using the Active Directory module is `Get-ADGroup`. While you can use a LDAP filter to search for a group, the `Get-ADGroup` cmdlet also lets you supply PowerShell expressions:

```
Get-ADGroup -Filter { Name -like "*Management*" }
```

For more information on how to accomplish these tasks through the Active Directory module, see [the module’s online help documentation](https://oreil.ly/JiH6q).

# 26.15 Get the Properties of a Group

## Problem

You want to get and list the properties of a specific security or distribution group.

## Solution

To list the properties of a group, use the `[adsi]` type shortcut to bind to the group in Active Directory, and then pass the group to the `Format-List` cmdlet:

```
$group =
  [adsi] "LDAP://localhost:389/cn=Management,ou=West,ou=Sales,dc=Fabrikam,dc=COM"

$group | Format-List *
```

## Discussion

The Solution retrieves the `Management` group from the `Sales West` OU. By default, the `Format-List` cmdlet shows only the distinguished name of the group, so we type `Format-List *` to display all properties.

If you know the property for which you want the value, specify it by name:

```
PS > $group.Member
CN=SmithRobin,OU=West,OU=Sales,DC=Fabrikam,DC=COM
CN=MyerKen,OU=West,OU=Sales,DC=Fabrikam,DC=COM
```

If you’re having trouble getting a property that you know exists, you can also retrieve the property using the `Get()` method on the container. While the `name` property can be accessed using the usual property syntax, the following example demonstrates the alternative approach:

```
PS > $group.Get("name")
Management
```

The cmdlet to get the properties of a group using the Active Directory module is `Get-ADGroup`. For more information on how to accomplish these tasks through the Active Directory module, see [the module’s online help documentation](https://oreil.ly/JiH6q).

# 26.16 Find the Owner of a Group

## Problem

You want to get the owner of a security or distribution group.

## Solution

To determine the owner of a group, use the `[adsi]` type shortcut to bind to the group in Active Directory, and then retrieve the `ManagedBy` property:

```
$group =
  [adsi] "LDAP://localhost:389/cn=Management,ou=West,ou=Sales,dc=Fabrikam,dc=COM"

$group.ManagedBy
```

## Discussion

The Solution retrieves the owner of the `Management` group from the `Sales West` OU. To do this, it accesses the `ManagedBy` property of that group. This property exists only when populated by the administrator of the group but is fairly reliable: Active Directory administrators consider it a best practice to create and populate this property.

The cmdlet to find the owner of a group using the Active Directory module is `Get-ADGroup`. This cmdlet does not retrieve the `ManagedBy` property by default, so you also need to specify `ManagedBy` as the value of the `-Property` parameter. For more information on how to accomplish these tasks through the Active Directory module, see [the module’s online help documentation](https://oreil.ly/JiH6q).

# 26.17 Modify Properties of a Security or Distribution Group

## Problem

You want to modify properties of a specific security or distribution group.

## Solution

To modify a security or distribution group, use the `[adsi]` type shortcut to bind to the group in Active Directory. If the property has already been set, you can change the value of a property as you would with any other PowerShell object. If you’re setting a property for the first time, use the `Put()` method. Finally, call the `SetInfo()` method to apply the changes.

```
$group =
  [adsi] "LDAP://localhost:389/cn=Management,ou=West,ou=Sales,dc=Fabrikam,dc=COM"

PS > $group.Put("Description", "Managers in the Sales West Organization")
PS > $group.SetInfo()
PS > $group.Description
```

## Discussion

The Solution retrieves the `Management` group from the `Sales West` OU. It then sets the description to “Managers in the Sales West Organization,” and applies those changes to Active Directory.

The cmdlet to modify the properties of a security or distribution group using the Active Directory module is `Set-ADGroup`. For more information on how to accomplish these tasks through the Active Directory module, see [the module’s online help documentation](https://oreil.ly/JiH6q).

# 26.18 Add a User to a Security or Distribution Group

## Problem

You want to add a user to a security or distribution group.

## Solution

To add a user to a security or distribution group, use the `[adsi]` type shortcut to bind to the group in Active Directory, and then call the `Add()` method:

```
$management =
  [adsi] "LDAP://localhost:389/cn=Management,ou=West,ou=Sales,dc=Fabrikam,dc=COM"

$user = "LDAP://localhost:389/cn=MyerKen,ou=West,ou=Sales,dc=Fabrikam,dc=COM"
$management.Add($user)
```

## Discussion

The Solution adds the `MyerKen` user to a group named `Management` in the `Sales West` OU. To check whether you have added the user successfully, see [Recipe 26.20](#ch26.xhtml#list_a_users_group_membership).

The cmdlet to add a user to a security or distribution group using the Active Directory module is `Add-ADGroupMember`. For more information on how to accomplish these tasks through the Active Directory module, see [the module’s online help documentation](https://oreil.ly/JiH6q).

## See Also

[Recipe 26.20, “List a User’s Group Membership”](#ch26.xhtml#list_a_users_group_membership)

# 26.19 Remove a User from a Security or Distribution Group

## Problem

You want to remove a user from a security or distribution group.

## Solution

To remove a user from a security or distribution group, use the `[adsi]` type shortcut to bind to the group in Active Directory, and then call the `Remove()` method:

```
$management =
  [adsi] "LDAP://localhost:389/cn=Management,ou=West,ou=Sales,dc=Fabrikam,dc=COM"

$user = "LDAP://localhost:389/cn=MyerKen,ou=West,ou=Sales,dc=Fabrikam,dc=COM"
$management.Remove($user)
```

## Discussion

The Solution removes the `MyerKen` user from a group named `Management` in the `Sales West` OU. To check whether you have removed the user successfully, see [Recipe 26.20](#ch26.xhtml#list_a_users_group_membership).

The cmdlet to remove a user from a security or distribution group using the Active Directory module is `Remove-ADGroupMember`. For more information on how to accomplish these tasks through the Active Directory module, see [the module’s online help documentation](https://oreil.ly/JiH6q).

## See Also

[Recipe 26.20](#ch26.xhtml#list_a_users_group_membership)

# 26.20 List a User’s Group Membership

## Problem

You want to list the groups to which a user belongs.

## Solution

To list a user’s group membership, use the `[adsi]` type shortcut to bind to the user in Active Directory, and then access the `MemberOf` property:

```
$user =
  [adsi] "LDAP://localhost:389/cn=MyerKen,ou=West,ou=Sales,dc=Fabrikam,dc=COM"
$user.MemberOf
```

## Discussion
