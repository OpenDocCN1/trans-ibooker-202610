
> “A must-read for anyone in API security.”
> —Corey Ball, author of *Hacking APIs*

# Black Hat GraphQL

Attacking Next Generation APIs

![](img/2f244b00cd9f8f37cfc22f928aaa471a_0_0.png)

Nick Aleks and Dolev Farhi

Foreword by Opheliar Chan

![](img/2f244b00cd9f8f37cfc22f928aaa471a_0_1.png)

# BLACK HAT GRAPHQL

# BLACK HAT
GRAPHQL

# Attacking
Next Generation APIs

by Nick Aleks and Dolev Farhi

![](img/2f244b00cd9f8f37cfc22f928aaa471a_4_0.png)

no starch
press

San Francisco

# BLACK HAT GRAPHQL. Copyright © 2023 by Nick Aleks and Dolev Farhi.

All rights reserved. No part of this work may be reproduced or transmitted in any form or by any means, electronic or mechanical, including photocopying, recording, or by any information storage or retrieval system, without the prior written permission of the copyright owner and the publisher.

First printing

27 26 25 24 23     1 2 3 4 5

ISBN-13: 978-1-7185-0284-0 (print)
ISBN-13: 978-1-7185-0285-7 (ebook)

Publisher: William Pollock
Managing Editor: Jill Franklin
Production Manager: Sabrina Plomitallo-González
Production Editor: Jennifer Kepler
Developmental Editor: Frances Saux
Cover Illustrator: Rick Reese
Interior Design: Octopod Studios
Technical Reviewer: Corey Ball
Copyeditor: Sharon Wilkey
Compositor: Maureen Forys, Happenstance Type-O-Rama
Proofreader: James Fraleigh

For information on distribution, bulk sales, corporate sales, or translations, please contact No Starch Press, Inc. directly at info@nostarch.com or:

No Starch Press, Inc.
245 8th Street, San Francisco, CA 94103
phone: 1.415.863.9900
www.nostarch.com

Library of Congress Control Number: 2022046393

No Starch Press and the No Starch Press logo are registered trademarks of No Starch Press, Inc. Other product and company names mentioned herein may be the trademarks of their respective owners. Rather than use a trademark symbol with every occurrence of a trademarked name, we are using the names only in an editorial fashion and to the benefit of the trademark owner, with no intention of infringement of the trademark.

The information in this book is distributed on an "As Is" basis, without warranty. While every precaution has been taken in the preparation of this work, neither the authors nor No Starch Press, Inc. shall have any liability to any person or entity with respect to any loss or damage caused or alleged to be caused directly or indirectly by the information contained in it.

## About the Authors

Nick Aleks is a leader in Toronto’s cybersecurity community and a distinguished and patented security engineer, speaker, and researcher. He is currently the senior director of security at Wealthsimple; leads his own security firm, ASEC.IO; and is a senior advisory board member for HackStudent, George Brown College, and the University of Guelph’s Master of Cybersecurity and Threat Intelligence program. A founder of DEFCON Toronto (DC416), he specializes in offensive security and penetration testing and has over 10 years of experience hacking everything from websites to safes, locks, cars, drones, and even smart buildings.

Dolev Farhi is a security engineer and author with extensive experience leading security engineering teams in the fintech and cybersecurity industries. Currently, he is a distinguished security engineer at Palo Alto Networks, building defenses for the largest cybersecurity company in the world. He has worked for several fintech and security firms and provided training for official Linux certification tracks. He is also one of the founders of DEFCON Toronto (DC416), a popular Toronto-based hacker group. In his spare time, he enjoys researching vulnerabilities in IoT devices, building open source offensive security tools, participating in and building CTF challenges, and contributing exploits to Exploit-DB.

## About the Technical Reviewer

Corey Ball is the author of Hacking APIs (No Starch Press, 2022) and senior manager of penetration testing at Moss Adams. He has over 12 years of experience working in IT and cybersecurity across several industries. He is the creator of the APIsec University, a free resource where anyone can learn about API security. In addition to a bachelor’s degree in English and philosophy from Sacramento State University, he holds the OSCP, CCISO, CISSP, and several other certifications.

## BRIEF CONTENTS

- Foreword ........................................................... xv
- Acknowledgments ..................................................... xix
- Introduction ......................................................... xxi
- Chapter 1: A Primer on GraphQL ......................................... 1
- Chapter 2: Setting Up a GraphQL Security Lab ............................. 21
- Chapter 3: The GraphQL Attack Surface ................................... 41
- Chapter 4: Reconnaissance .............................................. 71
- Chapter 5: Denial of Service ........................................... 101
- Chapter 6: Information Disclosure ...................................... 139
- Chapter 7: Authentication and Authorization Bypasses .................... 163
- Chapter 8: Injection .................................................. 187
- Chapter 9: Request Forgery and Hijacking ............................... 221
- Chapter 10: Disclosed Vulnerabilities and Exploits ...................... 247
- Appendix A: GraphQL API Testing Checklist .............................. 269
- Appendix B: GraphQL Security Resources ............................... 273
- Index ............................................................... 275

## CONTENTS IN DETAIL

- FOREWORD                                                                 XV
- ACKNOWLEDGMENTS                                                         XIX
- INTRODUCTION                                                            XXI
  - Who This Book Is For . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . xxii
  - The Book’s Lab and Code Repository . . . . . . . . . . . . . . . . . . . . . . . . . . xxii
  - What’s in This Book . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . xxii

- 1
- A PRIMER ON GRAPHQL                                                                     1
  - The Basics . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 1
    - Origins . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 2
    - Use Cases . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 2
    - Specification . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3
  - How Do Communications Work? . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3
    - The Schema . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 4
    - Queries . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 6
    - The Query Parser and Resolver Functions . . . . . . . . . . . . . . . . . . . . . . 7
  - What Problems Does GraphQL Solve? . . . . . . . . . . . . . . . . . . . . . . . . . . . 8
  - GraphQL APIs vs. REST APIs . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 9
    - The REST Example . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 10
    - The GraphQL Example . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 12
    - Other Differences . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 15
  - Your First Query . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 17
  - Summary . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 20

- 2
- SETTING UP A GRAPHQL SECURITY LAB                                                      21
  - Taking Security Precautions . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 22
  - Installing Kali . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 23
  - Installing Web Clients . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 24
    - Querying from the Command Line with cURL . . . . . . . . . . . . . . . . . . . . . 25
    - Querying from a GUI with Altair . . . . . . . . . . . . . . . . . . . . . . . . . . . 25
  - Setting Up a Vulnerable GraphQL Server . . . . . . . . . . . . . . . . . . . . . . . . 28
    - Installing Docker . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 28
    - Deploying the Damn Vulnerable GraphQL Application . . . . . . . . . . . . . . . 29
    - Testing DVGA . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 31
  - Installing GraphQL Hacking Tools . . . . . . . . . . . . . . . . . . . . . . . . . . . . 31
    - Burp Suite . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 32
    - Clairvoyance . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 33
    - InQL . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 34
    - Graphw00f . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 35
    - BatchQL . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 36
