information in JSON format, so we use the json() method to convert the information to a Python dictionary ⑤. We assign the resulting dictionary to response_dict.

Finally, we print the keys from response_dict and see the following output:

```
Status code: 200
dict_keys(['total_count', 'incomplete_results', 'items'])
```

Because the status code is 200, we know that the request was successful. The response dictionary contains only three keys: 'total_count', 'incomplete_results', and 'items'. Let’s take a look inside the response dictionary.

# Working with the Response Dictionary

With the information from the API call represented as a dictionary, we can work with the data stored there. Let’s generate some output that summarizes the information. This is a good way to make sure we received the information we expected, and to start examining the information we’re interested in:

python_repos.py

```
import requests

# Make an API call and store the response.
--snip--

# Convert the response object to a dictionary.
response_dict = r.json()
① print(f"Total repositories: {response_dict['total_count']}")
print(f"Complete results: {not response_dict['incomplete_results']}")

# Explore information about the repositories.
② repo_dicts = response_dict['items']
print(f"Repositories returned: {len(repo_dicts)}")
```