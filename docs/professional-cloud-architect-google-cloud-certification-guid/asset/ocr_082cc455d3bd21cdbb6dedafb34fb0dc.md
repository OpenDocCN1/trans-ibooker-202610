![](bbox=[0.22, 0.09, 0.74, 0.34])

# Figure 16.16 – New VM instance

We can also specify the variable properties in our gcloud command. In the following example, we have a new template called vm-template1.jinja, which is expecting variables for a hostname and a zone:

```
resources:
- name: {{ properties["name"] }}
  type: compute.v1.instance
  properties:
    zone: {{ properties["zone"] }}
    machineType: zones/{{ properties ["zone"] }}/machineTypes/g1-small
    disks:
    - deviceName: boot
      type: PERSISTENT
      boot: true
      autoDelete: true
      initializeParams:
        sourceImage: projects/centos-cloud/global/images/centos-stream-8-v20210721
    networkInterfaces:
    - network: global/networks/default
    accessConfigs:
    - name: External NAT
      type: ONE_TO_ONE_NAT
```