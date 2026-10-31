3. With the -s option, we can search for specific strings, such as the OS name:

![](bbox=[0.19, 0.15, 0.81, 0.34])

# Understanding the services command

The services command allows us to display the services running on the hosts. To view the help for the services command, we can use the -h option:

```
msf > services -h
```

```
Usage: services [-h] [-u] [-a] [-r <proto>] [-p <port1,port2>] [-s <name1,name2>] [-o <filename>] [addr1 addr2 ...]
```

+   -a,--add          Add the services instead of searching
+   -d,--delete        Delete the services instead of searching
+   -c <col1,col2>     Only show the given columns
+   -h,--help          Show this help information
+   -s <name1,name2>   Search for a list of service names
+   -p <port1,port2>   Search for a list of ports
+   -r <protocol>      Only show [tcp|udp] services
+   -u,--up            Only show services which are up
+   -o <file>          Send output to a file in csv format
+   -O <column>        Order rows by specified column number
+   -R,--rhosts        Set RHOSTS from the results of the search
+   -S,--search        Search string to filter by

Available columns: created_at, info, name, port, proto, state, updated_at