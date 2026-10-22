**DEFINITION** NIST is another organization that has contributed greatly to the advancement of security. It is an organization based in the United States and is tasked with providing innovation and technical advancement. Through this effort NIST has defined many of the practices that are used not just in application security but in organizations who want to raise their overall security practices. (https://www.nist.gov/)

The Mozilla RRA (Rapid Risk Assessment) takes a similar approach to measuring risk as OWASP does in the sense that it aims to be discrete and quick. It looks at the risk from the point of view of whether the given platform has the appropriate level of security controls to host specific data. The input into an RRA is a data flow diagram (DFD) that includes the type of data that is used by the service being assessed. Additionally, an understanding or documentation, of how the service works. From here, the process is similar to a threat model (we’ll talk about these later) where basic discussions occur on the service and its purpose. Data is highlighted with attention on how it’s stored and used. Then a methodical approach is taken to review possible threat scenarios that focus on the confidentiality, integrity, and availability of the data. Once this is complete, recommendations are made on how best to provide protection.

Identifying risks allows the organization to prioritize and frame vulnerabilities that are presented. This means the organization can focus on the risks that have the biggest impact on what matters most to them.

# 2.8 Summary

+   - The CIA triad (confidentiality, integrity, and availability) is the foundation for every decision that is made in protecting data and ensuring that our systems are available when needed.
+   - Knowing your potential threat actors will assist in the definition of the appropriate level of protection that is needed. You don’t need military grade protection if your only adversary is a script kiddy.
+   - Attacks get stronger as the threat actors get better. Defenses need to align with the treats that they are protecting against.
+   - Organizations such as NIST and OWASP are great resources for standards, and projects to help with ensuring your applications build security in.
+   - The OWASP Risk Rating Methodology provides a means to define the risk posed to an organization through a calculation that takes into consideration the technical and business impacts as well as the threat actors.
+   - Risks can be remediated, mitigated, or accepted. Each have their own benefits and disadvantages.