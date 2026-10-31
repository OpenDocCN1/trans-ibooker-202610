# framework/config/database.yml

# Creating initial database schema

The msfdb command allows you to manage the Metasploit Framework database, not just initialize the database. To display all the msfdb options, run the command as follows:

```
root@kali:~# msfdb
```

# Manage a metasploit framework database

```
msfdb init # initialize the database
msfdb reinit # delete and reinitialize the database
msfdb delete # delete database and stop using it
msfdb start # start the database
msfdb stop # stop the database
```

3. To modify the database configuration file, we can edit the database.yml file located in /usr/share/metasploit-framework/config/database.yml:

```
root@kali:~# cat /usr/share/metasploit-framework/config/database.yml
development:
  adapter: postgresql
  database: msf
  username: msf
  password: 3HCNhAtdH6F9F2iGa4z3wJVoI7UK1Ot+MG1zuKjYzn4=
  host: localhost
  port: 5432
  pool: 5
  timeout: 5

production:
  adapter: postgresql
  database: msf
  username: msf
  password: 3HCNhAtdH6F9F2iGa4z3wJVoI7UK1Ot+MG1zuKjYzn4=
  host: localhost
  port: 5432
  pool: 5
  timeout: 5

test:
  adapter: postgresql
  database: msf_test
  username: msf
```