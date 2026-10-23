# Lateral network movement opportunities

Since VMs are deployed into customer-managed virtual networks in Azure, it is common for organizations to have connectivity from traditional networks in their organizations to Azure virtual networks. Being able to run commands on Azure VMs gives us the opportunity to discover services and resources in the networks that may be connected to them and to look for opportunities to exploit them.

The ability to run commands can also present opportunities to bypass defenses. In some situations, there may be different **multifactor authentication (MFA)** requirements for on-premises Azure access and remote Azure access. The authors have seen environments that require MFA from the internet but do not require it for connections made from trusted internal on-premises IPs.

After escalating privileges within an on-premises environment, dumping and cracking credentials for a tenant administrator account, it may be possible to directly log in to the Azure management interfaces from an on-premises system, or from an Azure VM.

# Tenant credential hunting

In some cases, organizations have privileged access workstations, or jump hosts, hosted in Azure. These are systems that administrators can use to perform privileged operations against their environments. We would look to gather cached access tokens or other credentials from these machines for reuse.

Later in this chapter, we will be covering how to identify a managed identity on an Azure VM. These identities can also be used to escalate into the Azure tenant from the VM by generating tokens from the metadata service. The generated token can then be used for gathering sensitive information from other services that you may not have had access to.

Now that we understand our goals, let's look at different exploit scenarios for achieving these goals.

# Exploiting Azure platform features with Contributor rights

With access to the Contributor role, we have a rich set of platform-level features that we can use to manage VMs in the environment. Since we are approaching these features as penetration testers, our use cases for these features may be slightly different than your average user.