In this chapter, we are going to cover the following main topics:

+   - Preparing for Contributor (PaaS) exploit scenarios
+   - Attacking storage accounts
+   - Pillaging keys, secrets, and certificates from key vaults
+   - Leveraging web apps for lateral movement and escalation
+   - Extracting credentials from automation accounts

# Preparing for Contributor (PaaS) exploit scenarios

To follow along with the exercises in this chapter, you will need to set up a user with Contributor permissions and some vulnerable workload configurations in your own Azure subscription. As in previous chapters, we have automated this using a PowerShell script that you can run from Azure Cloud Shell. Before proceeding, ensure that you have run the clean-up scripts to remove resources from previous chapters. This will help us avoid any script execution exceptions.

Here are the tasks that we will complete in this exercise:

+   - **Task 1**: Create a test user account with Contributor privileges.
+   - **Task 2**: Deploy vulnerable workloads for all the scenarios.

Let's get started:

+   1. Open a web browser and browse to the Azure portal at https://portal.azure.com. Sign in with your azureadmin credentials.
+   2. In the Azure portal, click on the Cloud Shell icon in the top-right corner. Select PowerShell:

![](bbox=[0.2, 0.71, 0.8, 0.75])

Figure 6.1 – Click the icon to open Cloud Shell