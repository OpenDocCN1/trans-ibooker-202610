
In general, if we have the Contributor (or Owner) role on a subscription, we should be able to access virtual machine resources in the virtual network with access to the connection. Worst-case scenario, we could always create a new VM in the appropriate virtual network or attach a network interface to an existing VM.

With access to an on-premises network, and some domain credentials, we will want to find any available options for escalating our privileges in the domain. We will cover some common options and tools in the following sections.

## Identifying domain escalation paths

Since we are assuming that we have escalated to an Owner role, or to an account with access to multiple subscriptions, we will assume that some domain credentials were gathered at some point along the way. If not, we would recommend revisiting previous chapters to review ways that we can extract domain credentials from virtual machines and PaaS services.

Using these acquired domain credentials, we will want to use tools to identify resources in the Active Directory domain that can be used to escalate our privileges. In many environments, multiple hops between systems will be required to gain access to domain administrator rights.

As an example, here is a common domain escalation path for an Active Directory domain (ACME.local) that is integrated with Azure resources:

- An attacker gathers domain credentials (Password Guessing, Network Protocol Attacks, and so on) from Active Directory or an Azure VM.
- The attacker uses the credentials to get a Kerberos ticket in a crackable format as part of a Kerberoasting attack.
- The ticket is cracked, revealing the cleartext credentials for the associated service account (ACME\SQLUser1).
- The service account has local administrator rights on the AD servers computer group.
- A server in the group (FileServer01) has an active logon session for the ACME\kfosaen-DA account, a Domain Administrator.
- The attacker uses the SQLUser1 service account credentials to log in to the FileServer01 server as a local administrator, and dumps the Domain Administrator credentials from the LSASS process with Mimikatz.

At the time of writing, this is a common escalation path that can be used in an Active Directory domain. This is a path that can be manually identified by gathering some basic domain information (service principals, domain groups, and so on), but there are tools available to help automate the identification of these paths. We will cover some of these tools in the next sections, along with additional tools that help with pivoting between systems.

## Automating the identification of escalation paths

The BloodHound tool was created to help simplify the identification of Active Directory escalation paths. The tool allows a domain user to gather critical information about the active hosts and user sessions in the domain to help chain together privileges into a graph. This graph can then be parsed to find the most direct path to the desired privileges:

![](img/4fa338e84e0a590967a142dd798ba4e2_294_0.png)

Figure 7.20 – Example BloodHound graph

Using our previous example, an attacker could collect the domain data and easily find a path from the Kerberoasting attack through domain administrator privileges by using BloodHound.

### BloodHound information

**Creators:** Andrew Robbins (Twitter: @wald0), Rohan Vazarkar (Twitter: @CptJesus), and Will Schroeder (Twitter: @harmj0y)

**Open Source or Commercial:** Open source project; commercial option available

**GitHub Repository:** `https://github.com/BloodHoundAD/BloodHound`

**Language:** PowerShell, JavaScript

Thanks to recent updates to the tool, BloodHound also supports finding many of the Azure attack paths mentioned in this chapter. The ability to graph the Azure roles and resources relationships can be extremely helpful in identifying ways to move laterally and escalate in Azure. While we won't cover the full usage of this tool, it's one of the most powerful tools for attacking Active Directory and highly recommended for Active Directory testing.

**Important note**

Since building up a lab for collecting the Azure data for BloodHound would require some specialized resource assignments in the subscription, we have opted to leave out the lab for this tool. If you want to see some of the data that it collects, feel free to run any of the build scripts from previous chapters, run the AzureHound collection tool, and then import the data into BloodHound.

While it is beyond the scope of this book, there are plenty of tools available to individually gather similar pieces of the information to those gathered by BloodHound. If done carefully, this piecemeal gathering of domain information may be less obvious than the BloodHound collection methods during a penetration test, but it will require more manual interpretation of the data (and specific knowledge of attack paths) to identify those same paths found by BloodHound.

## Tools for pivoting along escalation paths

With a clear path in mind for pivoting through the on-premises environment, we will need some tooling to help us automate some of this process to make our job easier. Thankfully, many common escalations can be completed from non-domain joined systems, without the need for custom tools. However, one of the best ways to maintain a low profile during an assessment is by living *off the LAN* by using native system management tools (RDP, PS Remoting, and so on).

Since native Windows tools don't have intrinsic support for things such as credential gathering and process migration, additional tools may be needed to complete our objectives. Since these tools could each have their own dedicated book, we will just highlight a few of them here and leave it up to the reader to learn these tools:

- Metasploit
- CrackMapExec
- Cobalt Strike
- Empire
- Red Team Toolkit
- Covenant

These tools can all be used as part of lateral movement and escalation in a domain. While the tools are not always needed, they can help simplify some of the efforts.

While this will always be influenced by the available paths in the environment, we will generally use the following methodology to pivot between systems in a domain environment during escalation:

- Obtain access to domain or local credentials.
- Use credentials or exploits to gain local administrator access to a system.
- Compromise available credentials on the system.
- Identify additional targets to pivot to.
- Move laterally to additional targets, and restart the cycle.

Once domain administrator credentials are gathered during the preceding process, we move into a new phase where we gather credentials for privileged Azure users and use those to pivot into the Azure tenant.

Next, we will cover what to do once we have escalated internally on the domain.

## General tips for post domain escalation and lateral movement

Domain administrator rights should not be the end of the line for an internal network penetration test. By stopping at domain administrator, you are showing that you have full control of the domain, but you aren't showing the potential impact of those rights. On an internal penetration test, an attacker can use these privileges to access sensitive data stores and drive home the potential dangers of an attacker escalating privileges.

The same methodology applies with an Azure penetration test. Getting domain administrator rights should really just be a foothold in working our way back up into the Azure AD environment as a privileged user. Since the Azure AD environments are so closely tied to the main Active Directory environments, there are many options for gaining control over Active Directory accounts that can allow us to pivot into Azure.

The first step after gaining domain administrative rights should be gathering domain NTLM hashes from a domain controller. This will open up the following opportunities for us:

- Cracking gathered hashes to obtain plaintext credentials
- Using the password hashes for **Pass-the-Hash (PtH)** attacks
- Persisting in the domain with golden ticket attacks (using the domain `krbtgt` account hash)

Since dumping all of the domain hashes can be a bit noisy, you may want to target specific accounts (krbtgt, domain/global administrators, and so on) for hash dumping. Additionally, those responsible for your domain may not appreciate having to reset every domain password, so make reasonable choices when dumping domain hashes.

For demonstration's sake, let's assume that we've gathered all of the domain NTLM hashes for the Global Administrator accounts in the Azure tenant. If we are able to crack any of these hashes, we may be able to use the cleartext credentials to pivot into the Azure tenant with a privileged role.

You may be thinking that all of these privileged accounts would be protected by MFA and conditional access policies. This is not always the case. While it is becoming more common to enforce MFA for all accounts, the authors have run into frequent scenarios where users are provisioned a privileged Azure AD role, but they never configure MFA for the account. Your mileage may vary, but it may be worth a try.

Alternatively, we can use the gathered domain credentials (cleartext or hashes) for privileged users to use for accessing systems used by Azure administrators. As previously mentioned, Azure tokens can be stored on user workstations in the `.Azure` profile folders. By accessing the workstations with one of the compromised privileged accounts, we may be able to assume the token of the Azure admin user. We will cover this process in depth in the following chapter as a method for obtaining persistence in the environment.

Finally, with administrative rights, we may have control over the MFA systems that are in use for the Azure environment. If third-party (Okta, Duo, and so on) MFA systems are in use, a privileged domain user may have control over the tokens for other users. By cracking a Global Administrator's password and granting them a new MFA token (on your device), you may be able to take over their Azure account.
