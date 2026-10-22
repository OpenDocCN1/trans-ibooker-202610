+   - Skill Level - How technically skilled is this group of threat agents? No technical skills (1), some technical skills (3), advanced computer user (5), network and programming skills (6), security penetration skills (9),
+   - Motive - How motivated is this group of threat agents to find and exploit this vulnerability? Low or no reward (1), possible reward (4), high reward (9)
+   - Opportunity - What resources and opportunities are required for this group of threat agents to find and exploit this vulnerability? Full access or expensive resources required (0), special access or resources required (4), some access or resources required (7), no access or resources required (9)
+   - Size - How large is this group of threat agents? Developers (2), system administrators (2), intranet users (4), partners (5), authenticated users (6), anonymous Internet users (9)

Vulnerability Factors:

+   - Ease of Discovery - How easy is it for this group of threat agents to discover this vulnerability? Practically impossible (1), difficult (3), easy (7), automated tools available (9)
+   - Ease of Exploit - How easy is it for this group of threat agents to actually exploit this vulnerability? Theoretical (1), difficult (3), easy (5), automated tools available (9)
+   - Awareness - How well known is this vulnerability to this group of threat agents? Unknown (1), hidden (4), obvious (6), public knowledge (9)
+   - Intrusion Detection - How likely is an exploit to be detected? Active detection in application (1), logged and reviewed (3), logged without review (8), not logged (9)

As you can see the likelihood factors are looking at the threat actors as it relates to the vulnerability. The organization can then take this information to determine how likely a vulnerability is to be exploited based on the skill level and knowledge of the vulnerability.

# 2.7.4 Estimating impact

Impact is a bit different when it comes to measuring as it is not based solely on what can be determined by just technical folks. Impact must leverage information related to the business considering that the impact is a measure of what is likely to happen to the organization should a vulnerability be exploited. One important consideration with impact is that there are two types of impact. Technical impact is a risk to our core security concerns of confidentiality, integrity, and availability. This focuses primarily on the systems that run and manage our application. Second is the business impact which prioritizes what is important for the business that is running the application and is usually financial in nature.

Technical Impact Factors: