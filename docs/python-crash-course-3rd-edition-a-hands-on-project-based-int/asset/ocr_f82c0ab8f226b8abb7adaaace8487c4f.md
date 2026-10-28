When we make a visualization for this data, we’ll want to include more than one repository. Let’s write a loop to print selected information about each repository the API call returns so we can include them all in the visualization:

# python_repos.py

```
```python
--snip--
# Explore information about the repositories.
repo_dicts = response_dict['items']
print(f"Repositories returned: {len(repo_dicts)}")

print("\nSelected information about each repository:")
for repo_dict in repo_dicts:
    print(f"\nName: {repo_dict['name']}")
    print(f"Owner: {repo_dict['owner']['login']}")
    print(f"Stars: {repo_dict['stargazers_count']}")
    print(f"Repository: {repo_dict['html_url']}")
    print(f"Description: {repo_dict['description']}")
```
```

We first print an introductory message ❶. Then we loop through all the dictionaries in `repo_dicts` ❷. Inside the loop, we print the name of each project, its owner, how many stars it has, its URL on GitHub, and the project’s description:

```
```text
Status code: 200
Total repositories: 248
Complete results: True
Repositories returned: 30

Selected information about each repository:

Name: public-apis
Owner: public-apis
Stars: 191494
Repository: https://github.com/public-apis/public-apis
Description: A collective list of free APIs

Name: system-design-primer
Owner: donnemartin
Stars: 179952
Repository: https://github.com/donnemartin/system-design-primer
```
```