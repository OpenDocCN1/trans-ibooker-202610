Even if a security control protected the API by banning clients from making more than, say, five HTTP login requests per minute, this attack would evade such logic, because we sent only a single HTTP request while performing 10 login attempts.

# Brute-Forcing Passwords with CrackQL

Manually building the large GraphQL document needed to successfully brute-force login credentials would be extremely time-consuming. In Chapter 2, you installed a GraphQL password brute-forcing and fuzzing tool called CrackQL. This tool accepts a single GraphQL query or mutation operation and automatically generates the alias payloads by using a CSV wordlist. Let's run the same password brute-force attack but, this time, use CrackQL to automate it.

Enter the CrackQL directory and then execute the brute-force attack against DVGA. The -t (target) argument specifies the destination GraphQL endpoint URL, the -q (query) argument takes a sample query (login.graphql), and the -i (input) argument defines the list of usernames and passwords to use in the attack. The --verbose argument allows us to view additional information such as the final payload before it is sent to DVGA.

```
# cd ~/CrackQL
# python3 CrackQL.py -t http://localhost:5013/graphql -q sample-queries/login.graphql -i sample-inputs/usernames_and_passwords.csv --verbose
```

CrackQL comes preinstalled with a sample username and password CSV dictionary, as well as the login.graphql query, shown in Listing 7-6. As you can see, it contains a single login mutation with two embedded variables, username and password. CrackQL uses Jinja-templating syntax, so variables are passed using double curly brackets ({{}}).

```
mutation {
    login(username: {{username|str}}, password: {{password|str}}) {
        accessToken
    }
}
```

Listing 7-6: The sample CrackQL login brute-force query

When you execute the CrackQL command, the tool will automatically take each username and password variable from the CSV file and inject them into a duplicated login operation in the same query document. CrackQL's verbose output provides payload details, as well as the output results:

```
Data:
[{'alias1': {'data': None,
    'inputs': {'password': 'admin', 'username': 'admin'}}},
--snip--

{'alias9': {'data': None,
    'inputs': {'password': 'operator', 'username': 'pass123'}}},
```