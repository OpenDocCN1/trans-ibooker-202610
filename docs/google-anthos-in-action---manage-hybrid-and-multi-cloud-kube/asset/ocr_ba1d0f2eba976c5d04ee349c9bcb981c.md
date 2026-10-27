quickly be validated by the Operations team administrators, they can be quickly deployed to the active repo.

In addition, several application teams have expressed interest in using GCP resources to offload some of the workloads for their application. Primarily, these teams are interested in Cloud SQL, Pub/Sub, and Storage buckets. Since the application teams have almost no one with experience using IaC tools, or with GCP in general, Evermore will be leveraging Config Connector to allow the teams to remain in the Kubernetes space for all deployment needs, as well as removing the need to configure and train users to access both Kubernetes and GCP itself.

Let us take a look at the repo outline:

```
<Git Repo Parent>
├─ bin
└─ policies
   ├─ namespace_selectors
   ├─ global
   │  ├─ rbac
   │  ├─ namespaces
   │  └─ policy_controller
   └─ teams
      ├─ griffins (accounting)
      │  ├─ rbac
      │  ├─ namespaces
      │  └─ applications
      │     ├─ reconciler
      │     ├─ expense_reports
      │     └─ accounts_pay_recv
      ├─ unicorns (public-facing stores and APIs)
      │  ├─ rbac
      │  ├─ namespaces
      │  └─ applications
      │     ├─ storefront
      │     ├─ order_cap_and_ful
      │     ├─ payment_auth
      │     └─ inventory
      └─ dragons (internal apps)
         ├─ rbac
         ├─ namespaces
         └─ applications
            ├─ timesheets
            └─ shipping_manager
```