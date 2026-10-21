# 7

# AUTHENTICATION AND AUTHORIZATION BYPASSES

![](bbox=[0.16, 0.5, 0.32, 0.65])

Out of the box, GraphQL has no authentication or authorization controls. As a result, the ecosystem has created its own or adopted those seen in traditional systems. In this chapter, we’ll cover the common GraphQL authentication and authorization implementations. Then we’ll discuss attacks that target some of their weaknesses.

*Authentication* is the mechanism by which a client proves their identity to a server. It answers the question: Is the user really who they say they are? Authentication attacks target a client’s identity, attempting to either steal credentials or spoof them to authenticate with a server, take certain actions on their behalf, or steal data to which they have access.

*Authorization* controls are responsible for granting access to data and ensuring that the actions an entity takes, whether they’re a human or a machine, match their assigned roles, groups, and permissions. Authorization attacks attempt to either bypass a security control entirely or poke holes in