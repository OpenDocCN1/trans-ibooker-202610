# Listing 9.15: Using if condition statements

```
$ cat test12.sh
#!/bin/bash
# testing the if condition
if [ $1 -eq $2 ]
then
  echo "Both values are equal!"
  exit
fi
if [ $1 -gt $2 ]
then
  echo "The first value is greater than the second"
  exit
fi
if [ $1 -lt $2 ]
then
  echo "The first value is less than the second"
  exit
fi
$
```

The test12.sh script shown in Listing 9.15 evaluates two values entered as parameters on the command line:

```
$ chmod u+x test12.sh
$ ./test12.sh 10 5
The first value is greater than the second
$
```

Only the command from the if statement that evaluated to a True logical value was processed by the shell script.

> You can combine tests by using the Boolean AND (&&) and OR (||) symbols.

# The case Statement

Often you'll find yourself trying to evaluate the value of a variable, looking for a specific value within a set of possible values, similar to what we demonstrated in Listing 9.15. Instead of having to write multiple if statements testing for all of the possible conditions, you can use a case statement.