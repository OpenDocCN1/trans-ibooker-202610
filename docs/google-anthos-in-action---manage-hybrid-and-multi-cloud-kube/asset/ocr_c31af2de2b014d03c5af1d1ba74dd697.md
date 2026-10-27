| Process Option | Results of Exception |
|---|---|
| audit | Namespace(s) will not report Audit results, but will still be part of the webhook and sync processes. |
| webhook | Namespace(s) will be exempt from the admission controller, but will still be part of the audit and sync processes. |
| sync | Namespace(s) resources will not be reported into Gatekeeper, but will still be part of the audit and webhook processes. |
| * | Exempts the namespace(s) from all Gatekeeper processes. |

For example, we may want to exempt a few namespaces from all Gatekeeper processing. To exempt all processes, we can create a new Config that contains the namespaces, with a * in the processes field. The manifest shown below will create a config that exempts the kube-system and gatekeeper-system namespaces from all Gatekeeper processes.

```
apiVersion: config.gatekeeper.sh/v1alpha1
kind: Config
metadata:
  name: config
  namespace: "gatekeeper-system"
spec:
  match:
    - excludedNamespaces: ["kube-system", "gatekeeper-system"]
      processes: ["*"]
```

You can add different process exemptions for different namespaces by adding additional matches. For example, you can create a match that only exempts a namespace from the webhook process, while another namespace