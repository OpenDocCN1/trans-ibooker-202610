```
```text
if(user) {
    user.id = identifier;
    user.fname = givenName;
    user.lname = familyName;
    user.email = email;
    user = data.write(user);
}
return user;
}
```
```

There may also be scenarios with the interface has external actions that do not map directly to internal functions. Consider the following external action:

```
```html
<form name="declineContract" method="post" action="...">
    <input name="customerId" value="q1w2e3r4t5" />
    <input name="contractId" value="o9i8u7y6.t5r4" />
    <input name="salesRepName" value="Mandy Miningham" />
    <input name="reviewerName" value="Mark Morkelsen" />
    <input name="reasonCode" value="0201.B" />
    <textarea name="comments">Unable to locate
collateral</textarea>
    <input type="submit" value="Decline Contract" />
</form>
```
```

There is no `declineContract` method in the underlying service code behind this interface. However, there are a handful of actions that must be completed when a contract is declined:

+   1. Create a `declined` log record
+   2. Update the `customer` record
+   3. Update the `contract` record
+   4. Update the `salesRep` record
+   5. Update the `reviewer` record
+   6. Update the `nationalCredit` agency

In this case, the service API most likely has a set of functions it needs to perform against several dependent services. Some of these functions might be local (e.g. internal objects in the service code) and some might be remote