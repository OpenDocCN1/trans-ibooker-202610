However, that is subject to race conditions, and is obviously a simple pattern. It is also partly predictable, but sometimes you want to have a clue as to what code is cluttering up $TMP. Don’t forget to set a trap (see “It’s a Trap!” on page 97) to clean up after yourself. We recommend you consider using mktemp, though that’s a large issue outside the scope of bash idioms.

![](bbox=[0.15, 0.21, 0.24, 0.28])

# $RANDOM and dash

$RANDOM is not available in dash, which is /bin/sh in some Linux distributions. Notably, current versions of Debian and Ubuntu use dash because it is smaller and faster than bash and thus helps to boot faster. But that means that /bin/sh, which used to be a symlink to bash, is now a symlink to dash instead, and various bash-specific features will not work. It does work in Zsh though.

# Command Substitution

We’ve already used command substitution quite a bit in Chapter 2, but we haven’t talked about it. The old Bourne way to do it is `` (backticks/backquotes), but we prefer the more readable POSIX $( ) instead. You will see a lot of both forms, because it’s how you pull output into a variable; for example:

```
unique_lines_in_file="$(sort -u "$my_file" | wc -l)"
```

Note that these are the same, but the second one is internal and faster:

```
for arg in $(cat /some/file)
for arg in $(</some/file)    # Faster than shelling out to cat
```

![](bbox=[0.15, 0.6, 0.24, 0.68])

# Command Substitution

Command substitution is critical to cloud and other DevOps automation because it allows you to gather and use all the IDs and details that only exist at runtime; for example:

```
instance_id=$(aws ec2 run-instances --image $base_ami_id ... \
    --output text --query 'Instances[*].InstanceId')
```

```
state=$(aws ec2 describe-instances --instance-ids $instance_id \
    --output text --query 'Reservations[*].Instances[*].State.Name')
```