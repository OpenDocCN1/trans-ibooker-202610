# FIGURE 3.70 Azure Relay and hybrid connection flow diagram

![](bbox=[0.18, 0.16, 0.82, 0.6])

Notice that connectivity between an Azure VM and on-premise network is not the only compute resource that can make a connection like this. Additionally, Azure Functions and Azure App Service can be configured to use Azure Relay via a hybrid connection.

There is one piece missing, which is the code. Both the client and the server require custom code to manage the authentication and connectivity with the endpoint. Using the Microsoft.Azure.Relay NuGet package, the connectivity can be configured. Explaining how to perform the coding is outside the scope of the book, and it's not required to know this for the Azure Solutions Architect Expert exam. However, knowing this capability exists and what the purpose is would be beneficial to understand.

A use case that works well in this scenario was illustrated in Figure 3.70, where you see a frontend that exists in Azure and the backend in an on-premise network. The frontend is an App Service web app that exposes an interface for placing orders, and the data source