
## Passive reconnaissance

Passive reconnaissance, as the name indicates, is a method of gathering information about the target individual and company by means of passive sources, without directly interacting with the intended target. This is the safest form of information gathering because there is no interaction with the target so it cannot be traced back to you. Passive reconnaissance includes gathering information from public sources. This could include gathering information available on the internet. Passive information itself is usually harmless, but combined with attack vectors, it can be exploited. For example, let's say that you visit the social media profile of the target and find out that the person is very interested in dogs. This information itself is not very useful. But if you send them an email containing a phishing link (phishing will be explained in a moment) containing some information about dogs, it is more likely that the target will click on this link and eventually compromise the system. Passive reconnaissance is usually done through search engines and public databases. Passive reconnaissance is much slower and usually gives limited technical data about the target. Although it is slower, the risk of getting caught in passive reconnaissance is very low.

## Active reconnaissance

In active reconnaissance, you engage with the target directly, either personally or through the computer. Active reconnaissance is much faster and gives a lot of information about the target, albeit at the cost of higher risk. Active reconnaissance includes finding information about the system used by the target as well as other technical specifications associated with the target's system. The following is a list of the most used information sought in active reconnaissance; however, this is not a comprehensive list:

- **IP address**: The internet protocol address of the target, both private and public.
- **MAC address**: Field identifying the hardware interface used by the target to connect to the network.
- **Ports**: Port scanning is one of the most frequently used tools in active reconnaissance. Open ports in the system can be used for initiating a connection with the target without their knowledge.
- **Services/software running on the target machine**: Having knowledge of the different services running on a target could be a good starting point for initiating attacks. If a service running on the target has a known vulnerability, it could be easily exploited.
- **Operating system fingerprinting**: Determining the operating system used by the target.

## Identifying weaknesses

Network scanning and reconnaissance would give you a lot of information. It is necessary that you keep track of all the information obtained in a structured manner, which would help you to identify relevant information. In practical cases, hackers work on information gathering for an extended period lasting from a few months to even years. Once you are confident that you have sufficient information, you can proceed forward to the next step, which is identifying weaknesses. This step includes examining all the information obtained in the previous step and determining which information could be useful for carrying out an attack.

## Attacking and gaining access

Once you have identified the weaknesses, the next step is to start team are the individuals whose goal is to limit the extent of the attack and provide restoration operations to the services. Once the hacker achieves their objectives, they should cover their tracks completely; otherwise, they can be easily detected by forensics. Common methods of covering tracks include removing logs and temporary files created during the attack phase and cleaning registry entries, caches, and in some cases browser history. A penetration tester should also be aware of logging mechanisms related to different operating systems. For example, the Windows operating system maintains the record of recently accessed and modified files using *jump lists*. Digital forensic experts use these technologies to determine the attacker and the extent of the attack on the system.

A lot of open source tools are available on the internet for covering tracks that perform a very good job at hiding your identity. For example, in *Metasploit*, you can use scripts such as `clearv` to clear up all event logs on Windows machines.

Another method to cover tracks is by using *reverse HTTP shells*. A shell is a code that executes user commands on a system. We will talk more about this in later chapters. In most computers, port 80 is used for HTTP packets; therefore, port 80 is open a lot of time in computers. It is very hard for firewalls to distinguish between legitimate and malicious packets over port 80. Using HTTP-based reverse shells, forensic analysts have a very hard time distinguishing hackers.

Once the hacker has gained access to the system, they will run various commands over the command-line interface. Once the objective is achieved, the hacker usually deletes the command history in order to avoid detection. This is done using the `export HISTSIZE=0` command in Linux-based systems.

## Reporting

The last phase of penetration testing or ethical hacking is to compile a report about all the weaknesses of the system as well as the achieved objectives of the penetration test. The pen-test report should list out all the necessary details regarding the attack. A penetration test report usually contains the following items.
