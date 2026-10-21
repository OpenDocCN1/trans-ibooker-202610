As you can see, the disclosed information isn’t very sensitive in nature, but it can be used to infer whether the program is private and, therefore, find HackerOne’s customers.

HackerOne paid a bounty of $2,500 for this authorization issue because of the information disclosure impact.

# Reading Private Notes (GitLab)

Issues created on GitLab may include private notes that only members should be able to view. In June 2019, an ethical hacker with the handle ngalog reported CVE-2019-15576 through a HackerOne report (https://hackerone.com/reports/633001), which showed that hackers can read these notes through GitLab’s GraphQL API despite them being properly restricted in the REST API.

Notes can be sensitive, as they may contain information about duplicate issues, issues moved to another project, or even project code. The ethical hacker used the following query to exploit the vulnerability:

```
query {
  project(fullPath:"username16/ci-test"){
    issue(iid:"1"){
      descriptionHtml
      notes {
        edges {
          node {
            bodyHtml
            system
            author {
              username
            }
            body
          }
        }
      }
    }
  }
}
```

As you can see, the issue object is being used in conjunction with the notes field. This notes field allows access to other fields, such as the note’s body, the note’s author, and more. The screenshot in Figure 10-4, taken from GitLab GraphQL API documentation, shows the complete list of available fields.