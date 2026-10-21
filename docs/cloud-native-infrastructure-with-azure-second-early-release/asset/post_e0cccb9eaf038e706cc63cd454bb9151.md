
```
location = azurerm_resource_group.rg.location
sku = "Premium"

identity {
    type = "UserAssigned"
    identity_ids = [
azurerm_user_assigned_identity.example.id ]
}

encryption {
    enabled = true
    key_vault_key_id = data.azurerm_key_vault_key.example.id
    identity_client_id =
azurerm_user_assigned_identity.example.client_id
}
}

resource "azurerm_user_assigned_identity" "example" {
    resource_group_name = azurerm_resource_group.example.name
    location = azurerm_resource_group.example.location
    name = "registry-uai"
}

data "azurerm_key_vault_key" "example" {
    name = "super-secret"
    key_vault_id = data.azurerm_key_vault.existing.id
}
```

## Storing Docker Images in a Registry

As we discussed previously, you need to be able to store and serve container images reliably. Once you’ve built your custom image, you need to push it to the registry. Also, and as we discussed previously, there are numerous options for storing images, including the Docker Registry and the container registries of the various cloud providers (e.g., Azure Container Registry). In this case, we are going to utilize the Harbor registry we created earlier in this chapter.

First we will log in to the registry:

```
$ docker login <harbor_address>
```

For example:

```
$ docker login https://myharborinstallation.com
```

Now we’ll tag the image:

```
docker tag example-flask-container <harbor_address>/demo/example-flask-container
```

And now we’ll push it:

```
docker push <harbor_address>/demo/example-flask-container
```

Once this completes, Harbor will be able to serve our image to any container software that is downloading it.

## Running Docker on Azure

In Azure, there are two ways you can run your Docker container image: using Azure Container Instances or running your own virtual machine with Docker installed. In this section, we will demonstrate both of these methods.

### Azure Container Instances

Azure Container Instances (ACI) allows you to quickly spin up a container instance in Azure without having to manage underlying infrastructure. This provides a Docker-like service, but in the public cloud. ACI allows the deployment of Azure Container Registry images or Docker images from either private or public repositories.

ACI also allows these instances to be placed in private or public networks depending on the use of the container images.

Some additional features of ACI include:

- No need to configure an OCI runtime specification
- The ability to pass environment variables to the container
- Basic firewall management
- Managed container restart policies

Simply put, ACI is the easiest way to start running containers in Azure! ACI also gives you the ability to test new container images without creating a totally separate environment.

There are some important caveats to note when running an Azure ACI:

- There is a quad-core/16 GB limit to any single container instance.
- The ACI settings for CPU and memory will override any runtime setting you applied to your container image.

### Deploying an Azure Container Instance

Deploying an Azure container instance is exceptionally simple. First, log in to the Azure portal and then click “Create a resource,” as shown in Figure 3-8.

![](img/8d47d1b5dbca55e784ae602145a894c7_95_0.png)

Figure 3-8. Azure portal home page

Click down to Containers on the lefthand side and you’ll see an offering of container-related products (see Figure 3-9). Click the Container Instances link.

![](img/8d47d1b5dbca55e784ae602145a894c7_96_0.png)

Figure 3-9. Searching for container products

You will be presented with a form to fill out the basic information about the container instance you will be creating (Figure 3-10).

### Create container instance

Basics Networking Advanced Tags Review + create

Azure Container Instances (ACI) allows you to quickly and easily run containers on Azure without managing servers or having to learn new tools. ACI offers per-second billing to minimize the cost of running containers on the cloud.

Learn more about Azure Container Instances

# Project details

Select the subscription to manage deployed resources and costs. Use resource groups like folders to organize and manage all your resources.

Subscription *

Visual Studio Enterprise

Resource group *

Create new

# Container details

Container name *

Region *

(US) East US

Image source *

Quickstart images

Azure Container Registry

Docker Hub or other registry

Image *

mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)

Size *

1 vcpu, 1.5 GiB memory, 0 gpus

Change size

Review + create

< Previous

Next : Networking >

Figure 3-10. Basic configuration for an Azure container instance

Fill in the following fields:

### Subscription

Pick the subscription that you want to deploy into.

### Resource group

You can use an existing resource group or create a new one. In this case, we’re going to create a new one called rg-aci-test.

### Container name

Name your container instance. We are going to call ours `aci-demo`.

### Region

Choose your region. We are going to deploy in the East US region.

### Image source

This is where you pick your container image. You are presented with three options:

- Use a demo quickstart image that Azure provides.
- Use an image that has been uploaded to the Azure Container Registry.
- Use an image that is uploaded to Docker Hub or another registry. If you have deployed Harbor, you can configure this here. In this case, we’re going to pick the Nginx quickstart image (*mcr.microsoft.com/oss/nginx/nginx:19.15-alpine*).

### Size

Here you pick what kind of virtual machine you want. Keep in mind that the larger the virtual machine is, the more you will need to pay to run it. In this case, we are going to run a configuration of “1 vcpu, 1.5GiB memory, 0 gpus”. This means we have one virtual core, 1 GB of memory, and no graphics card.

Once this has been configured, click the Next: Networking button to configure the networking (Figure 3-11).

### Create container instance

Basics Networking Advanced Tags Review + create

Choose between three networking options for your container instance:

- 'Public' will create a public IP address for your container instance.
- 'Private' will allow you to choose a new or existing virtual network for your container instance. This is not yet available for Windows containers.
- 'None' will not create either a public IP or virtual network. You will still be able to access your container logs using the command line.

Networking type Public Private None

DNS name label  aci-demo .eastus.azurecontainer.io

Ports

| Ports | Ports protocol |
| 80 | TCP |

Review + create < Previous Next : Advanced >

Figure 3-11. Network configuration for an Azure container instance

In this demo, we are going to assign a public IP to our container instance (so that we can access it from the internet) and then give the IP address a DNS name of aci-demo.eastus.azurecontainer.io (you will want to use a unique name for this). We then have the option to make TCP or UDP ports available from outside the container. As we are using an Nginx container, port 80 is automatically opened. You can also create a virtual network (or use an existing one) and subnet if you’re using private IP addressing.

If you click the Next: Advanced button, you’ll see there are some other settings you can apply to deal with failure policies and environment variables. For this instance, we’re not going to make any changes (Figure 3-12).

### Create container instance

Basics Networking Advanced Tags Review + create

Configure additional container properties and variables.

Restart policy

On failure

Environment variables

Mark as secure Key Value

No

Command override

[ ]

Example: ["/bin/bash", "-c", "echo hello; sleep 100000"]

Review + create < Previous Next : Tags >

Figure 3-12. Advanced configuration for an Azure container instance

Finally, click “Review + create” and the container image will be created. It will take about two minutes for a quickstart image to deploy.

Once your container is deployed, you will be able to go to the DNS name of your container in an internet browser and see the “Welcome to nginx!” banner (see Figure 3-13). Via the Azure portal, you can also get a console connection to the instance.

# Welcome to nginx!

If you see this page, the nginx web server is successfully installed and working. Further configuration is required.

For online documentation and support please refer to nginx.org. Commercial support is available at nginx.com.

Thank you for using nginx.

Figure 3-13. Nginx default welcome page

You can find more information on deploying ACI in the Microsoft documentation.
