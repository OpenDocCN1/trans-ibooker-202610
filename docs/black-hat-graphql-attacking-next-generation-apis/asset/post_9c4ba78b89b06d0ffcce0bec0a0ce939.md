
They used the following GraphQL query as part of the test:

```
mutation {
  labelCreate(input:{title:"deactivated", projectPath:"test1/test1"}){
    errors
    label {
      id
    }
  }
}
```

The query uses the labelCreate object with an input type argument that accepts a title and a projectPath. In other words, the vulnerability allowed the ethical hacker to use a deactivated account to create a label field. It’s quite possible that the vulnerability would have allowed other actions too, other than label creation.

> NOTE The API token used in the exploit (not shown in the code snippet) was passed as a Bearer token to the Authorization HTTP header.

## Allowing an Unprivileged Staff Member to Modify a Customer’s Email (Shopify)

The following vulnerability was reported to the Shopify bug bounty program by user ash_nz in September 2021 (https://hackerone.com/reports/980511). An e-commerce company, Shopify has been a trailblazer in the GraphQL space for many years, developing useful open source tools, publishing articles about GraphQL best practices, and more.

The vulnerability allowed ash_nz to modify a customer email by using an unprivileged shop staff account, which could update email objects through a dedicated GraphQL API mutation. Here is the mutation as seen in the report:

```
mutation emailSenderConfigurationUpdate ($input:EmailSenderConfigurationUpdateInput!) {
  emailSenderConfigurationUpdate(input:$input) {
    emailSenderConfiguration {
      id
    }
    userErrors {
      field
      message
    }
  }
}
```

The hacker passed a customer’s email to the mutation’s input parameter and sent it to the GraphQL API server, which updated the customer’s email, despite the API caller not having the right privileges to do so.

This is a fairly simple vulnerability, but identifying it does require testing multiple hypotheses and edge cases. Always evaluate APIs using various
privilege levels and attempt cross-account or cross-user access to uncover authorization issues.

The hacker received a bounty of $1,500 from Shopify for responsibly disclosing this issue.

## Disclosing the Number of Allowed Hackers Through a Team Object (HackerOne)

In April 2018, an ethical hacker with the handle haxta4ok00 identified a GraphQL authorization issue in HackerOne that led to an information disclosure vulnerability (https://hackerone.com/reports/342978).

The hacker identified that, by making a query that uses the team object in HackerOne’s GraphQL API, they could access a restricted field that they otherwise shouldn’t have been able to access. The team object allowed querying for programs on the HackerOne platform and returning information such as their id and name.

The hacker also identified that when the whitelisted_hackers field is specified, it returns the total_count of the program’s number of allowed hackers. Since the team object takes an argument of handle, it practically allows searching for programs based on their handle string. In the following example, the handle is security:

```
query {
    team(handle:"security"){
        id
        name
        handle
        whitelisted_hackers {
            total_count
        }
    }
}
```

The HackerOne triage team was able to determine that this vulnerability could have also allowed someone to identify other non-public programs on the platform by supplying various strings to the handle argument that might match a team’s handle. The response to the query is as follows:

```
--snip--
"team":{
    "id":"Z2lkOi8vaGFja2Vybz5l1R1YwovMTM=",
    "name":"HackerOne",
    "handle":"security",
    "whitelisted_hackers":{
        "total_count":30
    }
}
--snip--
```

As you can see, the disclosed information isn’t very sensitive in nature, but it can be used to infer whether the program is private and, therefore, find HackerOne’s customers.

HackerOne paid a bounty of $2,500 for this authorization issue because of the information disclosure impact.

## Reading Private Notes (GitLab)

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

| Fields |  |  |
| Name | Type | Description |
| author | UserCore! | User who wrote this note. |
| body | String! | Content of the note. |
| bodyHtml | String | The GitLab Flavored Markdown rendering of note. |
| confidential | Boolean | Indicates if this note is confidential. |
| createdAt | Time! | Timestamp of the note creation. |
| discussion | Discussion | Discussion this note is a part of. |
| id | NoteID! | ID of the note. |

Figure 10-4: GitLab's documentation for the note fields

The full GitLab GraphQL API documentation can be found at https://docs.gitlab.com/ee/api/graphql/reference.

## Disclosing Payment Transaction Information (HackerOne)

The following vulnerability, reported to HackerOne in October 2019, impacted its own GraphQL API (https://hackerone.com/reports/707433). It allowed msdian7, the hacker who found and disclosed the issue, to access the total number of payment transactions—information meant to be confidential and accessible by only authorized parties.

The GraphQL query used can be seen here:

```
query ($handle_0: String!, $size_1: ProfilePictureSizes!) {
  team(handle: $handle_0) {
    id
    name
    about
    profile_picture(size: $size_1)
    offers_swag
    offers_bounties
    base_bounty
    payment_transactions {
      total_count
    }
  }
}
```

> NOTE The original query in the report used GraphQL fragments. For the sake of brevity, we merged the fragments with the query.

Payment data should never be public information. This vulnerability allowed access to the total_count field through the payment_transactions field by using an unauthorized session, effectively providing insight into the transactions made by other bug bounty programs on the HackerOne platform.

## Information Disclosure

In this section, we will review publicly disclosed vulnerabilities that led exclusively to information disclosure issues. Some of the issues we covered earlier in this chapter also resulted in information disclosure outcomes, though these stemmed from other vulnerabilities such as broken access control mechanisms.

## Enumerating GraphQL Users (GitLab)

In 2021, Rapid7 identified CVE-2021-4191 in GitLab’s Community Edition and Enterprise Edition. The vulnerability allowed unauthenticated attackers to access user information in private GitLab instances that had specifically restricted their user registration interfaces through the users field.

For example, the following query returns information about users in GitLab instances, such as their name, username, and ID:

```
query {
  users {
    nodes {
      id
      name
      username
    }
  }
}
```

In addition to a user’s name and username, the vulnerability affected fields such as their email, location, user permissions, group memberships, state, and profile picture. Having access to so much information about users is useful for several reasons:

- Identifying accounts to attack. Knowledge of usernames and emails allowed threat actors to target specific accounts. Having access to user emails also allowed threat actors to pivot to other attacks, such as social engineering, by sending phishing emails to users.
- Identifying available groups. The vulnerability allowed attackers to infer information about the company running GitLab through their group memberships. Group memberships can reveal information such as acquisitions, subsidiaries, other company branches, regions where the company operates, and so on.
- Identifying individuals. The vulnerability allowed access to profile pictures, which could help threat actors target specific users on platforms outside of GitLab.
- Identifying state of accounts. Knowing the state of an account (whether it’s disabled or enabled) could make attacks such as brute forcing more effective; threat actors could target only accounts that are in an enabled state, allowing them to optimize their attacks.
