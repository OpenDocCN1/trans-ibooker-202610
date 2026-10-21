```
$ ansible-playbook -vi hosts controllers.yaml
```

This will take some time to run; upon completion, you should see the following output:

```
PLAY RECAP
********************************************************************************
********************************************************************************
********************************************************************************
inventory_hostname=controller0 : ok=43    changed=39    unreachable=0    failed=0
skipped=4    rescued=0    ignored=0
inventory_hostname=controller1 : ok=22    changed=19    unreachable=0    failed=0
skipped=7    rescued=0    ignored=0
inventory_hostname=controller2 : ok=22    changed=19    unreachable=0    failed=0
skipped=7    rescued=0    ignored=0
```

This completes the setup of controller nodes. Essentially, with the *controllers.yaml* playbook we have bootstrapped the etcds and controller nodes. The Ansible playbook contains step-by-step instructions, which are self-explanatory, for bootstrapping the controller nodes for the cluster. We highly recommend that you follow the plays in the playbook for the execution path to understand the nitty-gritty details.

# Using Ansible to Deploy and Configure the Kubernetes Worker Nodes

Now that the controller nodes have been deployed, we can deploy the worker nodes using the *workers.yaml* file:

```
$ansible-playbook -vi hosts workers.yaml
.
.
.
PLAY RECAP
********************************************************************************
********************************************************************************
********************************************************************************
inventory_hostname=worker0 : ok=31    changed=29    unreachable=0    failed=0
skipped=20    rescued=0    ignored=0
inventory_hostname=worker1 : ok=23    changed=21    unreachable=0    failed=0
skipped=23    rescued=0    ignored=0
inventory_hostname=worker2 : ok=23    changed=21    unreachable=0    failed=0
skipped=23    rescued=0    ignored=0
```

This will deploy and configure the worker nodes with the necessary Kubernetes binaries.

# Setting Up Pod Networking and Routing

We still need to set up the Pod networking and routing for our cluster. To do this, we first need to set up the networking between the Pods by moving into the