+   - You can’t switch from an ES domain in a VPC to a public endpoint or vice versa. If you need to switch once an ES domain has been created, you will have to follow the ES migration process, typically using snapshots, which offer a convenient means to migrating your data.
+   - You can’t switch an ES domain’s VPC once it is created.
+   - You can’t apply IP-based policies to ES domains inside a VPC since the security groups already enforce IP-based access policies.

# Accessing Amazon Elasticsearch and Kibana

The key resource within Amazon Elasticsearch is an ES domain. Amazon Elasticsearch offers a number of ways to control and manage access to an ES domain. Amazon ES supports three types of policies:

+   - Resource based policies
+   - Identity-based policies
+   - IP-based policies

# Resource-Based Policies

The key resource within Amazon ES is an ES domain. You also have a domain’s sub-resources, like Elasticsearch indices and APIs. You can add resource-based policies, sometimes called domain access policies, when you create a domain. The policy specifies which actions a principal can perform on the domain’s sub-resources. The following policy grants analytics-user full access (es:*) to analytics-cert-domain.

```
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": [
                    "arn:aws:iam::987654321098:user/analytics-user"
                ]
            },
            "Action": [
                "es:*"
            ],
            "Resource": "arn:aws:es:us-west-1:123451234501:domain/analytics-cert-domain/*"
        }
    ]
}
```
```