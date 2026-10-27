|  |  |
|---|---|---|
|  | • Use existing corporate standards |
|  | • (ie: logging and monitoring standards) |
| Deployment Disadvantages |  |
| • Requires additional VMware licensing
• Customization of node appliance is not supported
• Limited GPU supported through passthrough
• Requires additional training for either the VM teams, or the Kubernetes support teams
• The vSphere scheduler and K8s scheduler are not aware of each other
• Storage DRS can break your cluster | • Requires planning to right size nodes to avoid wasted resources
• Does not include a storage provisioner other than local host storage
• Difficult to scale in most enterprises
• No node auto-healing |