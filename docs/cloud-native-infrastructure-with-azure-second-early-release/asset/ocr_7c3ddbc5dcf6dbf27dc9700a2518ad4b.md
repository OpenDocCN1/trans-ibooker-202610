```
"name": "azure",
"plugins": [
    {
        "type":"azure-vnet",
        "mode":"bridge",
        "bridge":"azure0",
        "ipam":{
            "type":"azure-vnet-ipam"
        }
    },
    "ipam": {
        "type": "azure-vnet-ipam",
        "Environment": "azure"
    },
    "dns": {
        "nameservers": [ "10.1.0.1" ]
    }
    },
    {
        "type": "tuning",
        "sysctl": {
            "net.core.somaxconn": "500"
        }
    }
]
}
```

Use of the azure-vnet and azure-vnet-ipam plug-ins moves IP configuration and management away from the host and to the Azure VNet control plane.

# Various CNI Projects

The CNI project contains a set of default CNI plug-ins split across three categories:

*Main*

These plug-ins create interfaces (bridge, ipvlan, macvlan, ptp, host-device).

*IPAM*