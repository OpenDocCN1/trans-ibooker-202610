```
// property for getting the identity of the user
// see below the Class Student
public: string IdentityOfTheUser()
{
string identity = "";

// implement the way of getting
// the identity of the user

return identity;
}

// property for getting the owners key
public: string OwnerSecretKey()
{
string secretKey = "";

// implement the way of queryying
// for secret key

return secretKey;
}

public: void AssignSecretKeyToUser()
{
AddUser u = new AddUser();
Student stud = new Student(u.OwnerSecretKey);
}
}

class Query
{
// Step 4
// the algorithm from Query step (algorithm)
// are runned and invoked by the user

// constructor of the class Query
public: Query() {}
```