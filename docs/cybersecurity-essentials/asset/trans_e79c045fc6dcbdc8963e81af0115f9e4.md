

Export-grade encryption was created to be good enough but still allow the National Security Agency to decrypt communication outside the United States. The NSA lifted this export requirement in the late 1990s, but those cipher suites have remained in both client and server libraries.

## FREAK Exploits

More than 15 years later, when a vulnerability known as *FREAK* (Factoring attack on RSA-EXPORT Keys) was discovered, it became clear that this export-grade feature was still available in many web clients, as well as in OpenSSL. This led to a scramble to patch the product.

While this still means an attacker would have to crack a 512-bit encryption, it is well known that this can be done in a matter of hours. However, 1024-bit encryption would take an impractically long time, and today encrypted services are often using 2048-bit encryption, which, for now anyway, seems uncrackable using contemporary technologies.

Because the FREAK flaw required a susceptible browser and server, its discovery also exposed another area of concern when it was learned just how many mobile browsers were susceptible. Many mobile platforms are simply not frequently updated, leading Microsoft, Apple, and Google to scramble and make software solutions available soon after the issue was discovered.

However, not all devices have upgrades available. Many older devices never had or no longer have software upgrade paths, so those browsers remain susceptible. The same can be said for the many painfully old desktop computers that are running obsolete operating systems, which will not support modern browsers. It is not that uncommon for employees to use personal mobile devices or laptops on their work networks, and these devices could create some risk for their organization.

## Logjam Exploits

Another popular cryptographic algorithm used in many Internet protocols rather than an RSA key exchange is the Diffie-Hellman key exchange (DHE). *A logjam attack* is similar to the FREAK attack but instead targets this Diffie-Hellman key exchange, convincing the connection to use DHE Export ciphers.

This attack exploits the *prime number sieve* (algorithm for determining all prime numbers up to a specified limit) used in the key-generation process, forcing it to use a 512-bit prime. Similar to FREAK, upgrade patches have been made available for modern browsers and software suites leaving obsolete systems vulnerable.

## Hands-On Exercises

### Objectives

- Create a new software restriction policy.
- Describe the difference between whitelisting and blacklisting.
- Create a new hash rule.

### Resources

- Customer-supplied desktop/laptop hardware system
- Windows 10 Professional installed (not the Home edition)
- An account with administrative access

### Discussion

Software-restriction policies provide another layer of defense by limiting the use of specified applications. Using the methods of whitelisting or blacklisting, you can successfully block attempts made by viruses, Trojans, and other malicious software from running a program without your knowledge.

Whitelisting is the practice of assigning access to the known good and denying all others, while blacklisting involves allowing everyone except those on the known bad list. In reference to applications, as with most listing options, whitelisting is the preferred and recommended method.

### Procedures

In this procedure, you will investigate the types of rules you can create to enforce software-restriction policies. You will test the new policy and new rule locally but understand that the same method can be used within a domain environment.

#### Locating Software-Restriction Policies

1. Power on your machine.
2. Log on using an account that has administrative access.
3. At the desktop, select the search bar. Type secpol.msc and then press Enter. The Local Security Policy MMC will launch, as shown in Figure 24.11.

![](img/2459befd842d6fed299fabe6da3512d0_628_0.png)

FIGURE 24.11 Local Security Policy

**NOTE:** These settings can also be accessed under the Local Group Policy Editor. Type gpedit.msc into the search bar and press Enter to launch.

**NOTE:** The MMC, or the Microsoft Management Console, provides an interface for users to configure the system. The MMC hosts tools and displays these tools in a console as a snap-in.

4. Click Software Restriction Policies to view the contents. The right pane should show No Software Restriction Policies Defined, as illustrated in Figure 24.12.

5. In the left pane, right-click Software Restriction Policies and select New Software Restriction Policies. The contents of the folder will be displayed in the right pane, as shown in Figure 24.13.

6. In the right pane, the bottom three object types can be adjusted by double-clicking each. Without adjusting any settings, explore Enforcement, as shown in Figure 24.14. When finished, click Cancel and then explore the Trusted Publishers and Designated File Types options.

![](img/2459befd842d6fed299fabe6da3512d0_629_0.png)

FIGURE 24.12 No Software Restriction Policies Defined

![](img/2459befd842d6fed299fabe6da3512d0_629_1.png)

FIGURE 24.13 Software Restriction Policies with Contents

![](img/2459befd842d6fed299fabe6da3512d0_631_0.png)

FIGURE 24.15 Security Levels

![](img/2459befd842d6fed299fabe6da3512d0_632_0.png)

FIGURE 24.16 Additional Rules

#### Creating and Testing a New Path Rule

Software Restriction Policies consist of a default security level and the associated exception rules that apply to group policies. Creating an exception rule is necessary when whitelisting or blacklisting.

1. In the search bar, type Notepad and press Enter. Notice that this executes the program.
2. Close Notepad without saving and return to the Local Security Policy MMC.
3. In the left pane of the Local Security Policy window, right-click Additional Rules. Select New Path Rule to launch the New Path Rule window, as shown in Figure 24.17.

![](img/2459befd842d6fed299fabe6da3512d0_633_0.png)

FIGURE 24.17 New Path Rule

A path rule will identify a program by its file path. For example, you can use this type of rule to prevent users from running email attachments, by specifying the mail program’s attachment folder.

4. Click Browse to launch the Browse For File Or Folder window. Locate Notepad by navigating to This PC → Local Disk (C:) → Windows → System32. Select Notepad, as depicted in Figure 24.18. (This will take a bit of scrolling down.)

![](img/2459befd842d6fed299fabe6da3512d0_634_0.png)

**FIGURE 24.18** Notepad Selected in Browse for File or Folder

5. Click OK to select Notepad and return to the New Path Rule window. The path to Notepad is located under Path.

6. Select the Security Level drop-down menu to view its choices. Leave it at Disallowed and click OK. The new rule should be located in the right pane of Additional Rules with a Security Level of Disallowed, as illustrated in Figure 24.19.

![](img/2459befd842d6fed299fabe6da3512d0_635_0.png)

**FIGURE 24.19** New Rule in Additional Rules

**NOTE:** The user must log off and log back on for the rules to apply.

7. Close the Local Security Policy MMC by clicking File and then selecting Exit.
8. Right-click the Windows/Start icon on the taskbar. On the menu, locate and hover your mouse over the Shut Down Or Sign Out option, and then on the options that are presented, select Sign Out.
9. Log back on with the same credentials you used at the beginning of this procedure.
10. Type Notepad into the search bar and press Enter.

Were you able to execute Notepad? ________

As shown in Figure 24.20, you should see the software restriction rule prompting a blocked message. This is considered blacklisting, as you blacklisted the program you didn’t want others to access.

![](img/2459befd842d6fed299fabe6da3512d0_636_0.png)

FIGURE 24.20 Notepad Blocked

11. Click Close to continue.
12. Return to the Local Security Policy MMC by typing secpol.msc in the search bar on your taskbar and pressing Enter.
13. Navigate to Software Restriction Policies > Additional Rules.
14. Locate the path rule you created and right-click it. Select Delete. When prompted, select Yes to permanently delete the selected rule.

#### Exploring the Rule Types

The remaining rule types accomplish the same output, in different ways. The four rule types include:

- Path Rule: Identifies software by its file path.

