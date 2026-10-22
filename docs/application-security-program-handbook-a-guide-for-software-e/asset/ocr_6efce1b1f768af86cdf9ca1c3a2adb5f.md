# 3.1.4 Threat modeling with linking bank accounts

We can use our examples from previous chapters of Superior Products who is launching a new feature in their e-commerce site that allows users to link their bank account in the application so that they can get paid for reselling items within the application. Dashing Danielle, the security representative in Superior Products for this application, begins by gathering the items needed as inputs from that development team. She reviews the architecture, and dataflow diagram as well as the software bill of materials to understand how the application is built and used. She follows up with the development team on a few outstanding questions and then organizes a work session with the development teams lead architect, two developers that have been working on the feature, and the product owner.

Dashing Danielle begins the session by drawing the simple architecture for the application along with, STRIDE, “Hollywood,” and the grid on a whiteboard. The group agrees that the architecture is complete and includes the critical components of the new feature. She then asks a basic question of “what are we trying to protect?” The obvious first answer is that the organization must protect the bank account information that is sent and stored. Dashing Danielle adds bank account information in the “What” column.

| What | Who | Why | How | Impact | Counter Measure |
|---|---|---|---|---|---|
| Bank Account Numbers |  |  |  |  |  |

Next Dashing Danielle ask, “who would want this information”? Similar to the question of “What”, there is little debate on “Who”. Clearly the attackers would be motivated by the financial gain that they would achieve by stealing bank account numbers. With this information, attackers would be able to access the bank accounts of the users of the application, gain additional information on the user, and potentially link the bank account to an attacker owned account. With this in mind, Dashing Danielle adds the information in to the “Who” and “Why”.

| What | Who | Why | How | Impact | Counter Measure |
|---|---|---|---|---|---|
| Bank Account Numbers | Cyber-criminals | Financial gains with the information. |  |  |  |

With the easy part done, it is now up for the group to determine how an attacker could steal bank account information and what impact it would have on the organization. Additionally, they will consider what counter measures they could put in place to protect against this specific attack, and whether those already exist. If they do already exist, then Dashing Danielle will cross it off the list under “Counter Measures”.