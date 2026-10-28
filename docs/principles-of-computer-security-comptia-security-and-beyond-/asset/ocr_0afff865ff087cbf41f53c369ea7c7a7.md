descriptions, as detailed at http://manual-snort-org.s3-website-us-east-1.amazonaws.com/node31.html.

| Keyword | Description |
|---|---|
| msg | The msg keyword tells the logging and alerting engine the message to print with the packet dump or alert. |
| reference | The reference keyword allows rules to include references to external attack identification systems. |
| gid | The gid keyword (generator id) is used to identify what part of Snort generates the event when a particular rule fires. |
| sid | The sid keyword is used to uniquely identify Snort rules. |
| rev | The rev keyword is used to uniquely identify revisions of Snort rules. |
| classtype | The classtype keyword is used to categorize a rule as detecting an attack that is part of a more general type of attack class. |
| priority | The priority keyword assigns a severity level to rules. |
| metadata | The metadata keyword allows a rule writer to embed additional information about the rule, typically in a key-value format. |

TABLE 13-1 General Rule Option Keywords from the Snort Manual

Let’s take another look at our Snort rule:

```
alert icmp any any -> $HOME_NET any (msg:"ICMP detected!"; sid:1000052; rev:1; classtype:icmp-event;)
```

The rule options section starts with the message “ICMP detected!,” which is the message that will be displayed in the alert and logged.

The sid (Snort ID) uniquely identifies Snort rules. IDs less than 100 are reserved for future use. IDs from 100 to 999,999 are rules