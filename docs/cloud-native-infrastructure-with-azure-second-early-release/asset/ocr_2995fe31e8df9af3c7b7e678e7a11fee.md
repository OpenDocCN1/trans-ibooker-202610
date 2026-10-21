n
t
m
o
d
e
s

| Mode | Description |
|------|-------------|
| Default | If a rule selects endpoints and the rule has an ingress of egress section, the endpoint will deny as per the policy. |
| Always | Policy enforcement is enabled on all endpoints, even if no rules select specific endpoints. |
| Never | Policy enforcement is disabled on all endpoints, and all traffic is allowed. |

To configure policy enforcement mode at runtime for all endpoints managed by a Cilium agent, use the following command and pick a mode of default, always, or never:

```
$ cilium config PolicyEnforcement={default,always,never}
```

Policy is written against an allow list model in which traffic will be blocked if there is no explicit allow rule.

L3 and L4 rules can be specified using the following methods:

# Labels

Use a selector to group Pods.

# Services

Define a policy using a Kubernetes service.

# Entities