archived
--snip--
url
visibility
watchers
watchers_count

At the time of this writing, there are only 248 Python repositories with over 10,000 stars ❶. We can see that GitHub was able to fully process the API call ❷. In this response, GitHub returned information about the first 30 repositories that match the conditions of our query. If we want more repositories, we can request additional pages of data.

GitHub's API returns a lot of information about each repository: there are 78 keys in repo_dict ❸. When you look through these keys, you'll get a sense of the kind of information you can extract about a project. (The only way to know what information is available through an API is to read the documentation or to examine the information through code, as we're doing here.)

Let's pull out the values for some of the keys in repo_dict:

***python_repos.py***

```
--snip--
# Examine the first repository.
repo_dict = repo_dicts[0]

print("\nSelected information about first repository:")
❶ print(f"Name: {repo_dict['name']}")
❷ print(f"Owner: {repo_dict['owner']['login']}")
❸ print(f"Stars: {repo_dict['stargazers_count']}")
print(f"Repository: {repo_dict['html_url']}")
❹ print(f"Created: {repo_dict['created_at']}")
❺ print(f"Updated: {repo_dict['updated_at']}")
print(f"Description: {repo_dict['description']}")
```