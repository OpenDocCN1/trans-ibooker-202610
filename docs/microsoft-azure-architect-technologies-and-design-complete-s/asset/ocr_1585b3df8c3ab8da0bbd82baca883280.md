The content of an ARM template is in the form of a JSON document. At the top, you will find a reference to a schema. Like a database schema, which textually describes the structure of a database, the JSON document schema defines the structure of the file and its supported attributes. There are a few attributes in the previous ARM template example that need some additional information, specifically, type, apiVersion, and dependsOn.

The type attribute is the resource that will be provisioned and configured by the template. These are considered resource providers, which are discussed later in the chapter in more detail. The value bound to the type attribute may look familiar to you already as it isn’t a new concept or structural pattern at this point. Read the “Resource Providers” section now if you want to dig deeper into this.

apiVersion identifies the version of the REST API used by the Azure Resource Manager to provision the product. (See Figure 8.26.) There is an exercise later in the chapter where you will use an ARM template to provision resources. When the ARM template is sent to the Azure Resource Manager, it is translated into an HTTPS request that is assembled using the JSON document contents. The type and apiVersion attributes would resemble something similar to the following:

```
PUT
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Web/sites/csharpguitar?api-version=2018-11-01
REQUEST BODY
{
  "location": "North Europe",
  "tags": { "Environment": "Production" },
  "kind": "app",
  "properties": { "httpsOnly": true }
}
```

For each type and respective configuration content for that type, an additional HTTPS request is sent to the resource provider for provisioning and configuration.

Now let’s return to the first sentence of this section where I stated this could become complicated. Consider if you have exported the template for all the products in a resource group. Think about how many HTTPS calls from the Azure Resource Manager to the resource provider per resource would be required to provision and deploy all those products, features, and configurations. With large ARM deployments, there are two topics that you should know about. The first one is the dependsOn attribute. As shown in the previous ARM template example, setting these App Service properties (autoHealEnabled, http20Enabled, minTlsVersion) is dependent on the existence of the app service. This makes sense because you cannot modify something that doesn’t yet exist. The same goes for binding a network interface to an Azure virtual machine or for adding an NSG to a subnet. Those configurations have dependencies on each other, and they will not succeed unless the required resources are available. It can be a challenge when some resources are successful, but the overall deployment fails partially through its execution. This is indeed