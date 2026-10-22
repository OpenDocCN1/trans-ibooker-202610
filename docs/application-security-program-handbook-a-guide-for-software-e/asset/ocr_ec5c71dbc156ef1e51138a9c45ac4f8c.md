This is where the team has to get creative. There are several ways an attacker can gain access to this information so it’s good to start with higher level themes. Here are a few cases they think about:

+   - Someone could accidentally, or intentionally, move the data to a developer environment for testing with live data.
+   - The data could be stolen directly in the database by an attacker who manages to deploy malicious code within the network to gain access to the database.
+   - The application could be susceptible to attacks that leak data like a SQL Injection, Cross-Site Scripting, or Cross-Frame Scripting.
+   - The bank account information could get logged to a logging system in plain text.

For the purposes of this exercise, the team decides to first focus on the ability of the attacker to take advantage of a weakness in the application by leveraging Cross-Frame Scripting (XFS). The other attack opportunities can be reviewed in sequence after the first one.

| What | Who | Why | How | Impact | Counter Measure |
|---|---|---|---|---|---|
| Bank Account Numbers | Cyber-criminals | Financial gains with the information. | Cross-Frame Scripting (XFS) |  |  |

# 3.1.5 What to do with the found threats

Dashing Danielle turns to the product owner to understand what the impact would be of a breach of this information with regards to any contractual stipulations that might require the company to pay a fee to clients. The product owner acknowledges that there are clear financial impacts directly linked with data loss, and also raises the concern of brand damage that might be difficult to overcome since there are other vendors and solutions in the market that are direct competitors with Superior Products application. Dashing Danielle also raises the likely support and recover cost associated with the attack as well as the potential for having to purchase credit monitoring for the impacted accounts.

| What | Who | Why | How | Impact | Counter Measure |
|---|---|---|---|---|---|
| Bank Account Numbers | Cyber-criminals | Financial gains with the information. | Cross-Frame Scripting (XFS) | Financial payments to clients, credit monitoring, support and recover costs, brand damage |  |

Since the team is not familiar with the specifics of an XFS attack, Dashing Danielle is able to describe it to the team. In this case, an attacker will use malicious JavaScript in an iframe