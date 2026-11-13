
# Hands-On Swift 5 Microservices Development

**Build microservices for mobile and web applications using Swift 5 and Vapor 4**

Ralph Kuepper

---

**BIRMINGHAM - MUMBAI**

---

Copyright © 2020 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing or its dealers and distributors, will be held liable for any damages caused or alleged to have been caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

**Commissioning Editor:** Kunal Chaudhari  
**Acquisition Editor:** Heramb Bhavsar  
**Content Development Editor:** Aamir Ahmed  
**Senior Editor:** Hayden Edwards  
**Technical Editor:** Sachin Sunilkumar  
**Copy Editor:** Safis Editing  
**Project Coordinator:** Kinjal Bari  
**Proofreader:** Safis Editing  
**Indexer:** Tejal Daruwale Soni  
**Production Designer:** Jyoti Chauhan

First published: February 2020  
Production reference: 1280220

Published by Packt Publishing Ltd.  
Livery Place  
35 Livery Street  
Birmingham  
B3 2PB, UK.

ISBN 978-1-78953-088-9

[www.packt.com](http://www.packt.com)

> To my wife and our two sons, for living life with them is a joy. – Ralph Kuepper

---

[Packt.com](https://subscribe.packtpub.com/)

Subscribe to our online digital library for full access to over 7,000 books and videos, as well as industry leading tools to help you plan your personal development and advance your career. For more information, please visit our website.

## Why subscribe?

- Spend less time learning and more time coding with practical eBooks and Videos from over 4,000 industry professionals
- Improve your learning with Skill Plans built especially for you
- Get a free eBook or video every month
- Fully searchable for easy access to vital information
- Copy and paste, print, and bookmark content

Did you know that Packt offers eBook versions of every book published, with PDF and ePub files available? You can upgrade to the eBook version at `www.packt.com` and as a print book customer, you are entitled to a discount on the eBook copy. Get in touch with us at `customercare@packtpub.com` for more details.

At `www.packt.com`, you can also read a collection of free technical articles, sign up for a range of free newsletters, and receive exclusive discounts and offers on Packt books and eBooks.

## Foreword

I had the pleasure of meeting Ralph in person at the first Server-Side Swift conference in Berlin in 2018. We met again at the next conference in Copenhagen. Ralph is quite a friendly person and we spent a lot of time getting to know each other and sharing ideas.

Ralph's experience studying and practicing business spanning over a decade have given him unique insights into the problems facing web developers today. In addition to his business acumen, he also has a deep understanding of programming. A man after my own heart, he's always eager to try new things and share his knowledge with others.

A few months before we met in person, Ralph's company, Skelpo, became Vapor's second major sponsor. This early support for the framework has proven to be invaluable. Beyond mere sponsorship, Ralph has been actively involved with the development of the framework. He is quick to adopt changes and give feedback. This feedback is critical to Vapor's success and shows his mastery of the framework.

What's more, Ralph is now well-known throughout the Vapor community. He has created many opportunities for contributors to work on related projects and give back to the framework. His work bolsters the community and helps the ecosystem to thrive. People like Ralph make open-source special and push software engineering forward.

I hope you enjoy the knowledge he has worked to compile and share with you in this book. Let it inspire you to try new things and share your knowledge with others.

**Tanner Nelson**  
Creator of Vapor

## Contributors

## About the author

**Ralph Kuepper** has worked in the web and software industry for over 15 years. He started his own company when he was 17 years old while still attending high school and college. Over the years, he has worked on projects for companies such as Adidas, KIA, and Honda. Coming from a traditional background of developing backends and websites using PHP and a monolithic approach, he has embraced and fine-tuned a Swift-based microservice approach for the last 4 years. His company, Skelpo Inc., has been actively involved in the development of the Vapor framework and has contributed a variety of open source microservices as well as related packages.

First, I would like to acknowledge my amazing best friend and wife, Elizabeth, for all the patience she had with me writing this book. Writing a book is a long and intense job and I appreciate all the support gave me in the process. I would also like to acknowledge the great staff and editorial team at Packt Publishing. Especially, I would like to thank Aamir Ahmed for his continual encouragement, direction, and support during the writing process. I'd also like to thank the technical reviewer, Caleb Kleveter, for providing invaluable insight and feedback from someone intimately familiar with Swift. Lastly, I want to thank Tanner Nelson, the founder of Vapor, for his dedicated support and, ultimately, writing the foreword of this book!

## About the reviewer

**Caleb Kleveter** is a contract software developer for Skelpo Inc. He has been working with Swift for the past 5 years since its release at WWDC 2014 and has been building microservice applications in Vapor for the past 3 years. Caleb also holds a maintainer status on several of the core Vapor repositories, has published several articles on Medium covering how to build backend applications using Vapor and the Fluent ORM, and has spoken at the ServerSide.swift conference on how to build resilient microservice applications in Vapor. Besides Swift on the server, Caleb also enjoys dabbling in other technologies, such as Vue.js, RegEx, and X86 Assembler, and spends his spare time writing streaming text parsers.

## Packt is searching for authors like you

If you're interested in becoming an author for Packt, please visit `authors.packtpub.com` and apply today. We have worked with thousands of developers and tech professionals, just like you, to help them share their insight with the global tech community. You can make a general application, apply for a specific hot topic that we are recruiting an author for, or submit your own idea.

## Table of Contents

1. [Title Page](#9ae08bcf-0b63-4ef2-acf1-59eea038416c.xhtml)
2. [Copyright and Credits](#3fad0a84-48a1-4476-a101-41c82da2a76e.xhtml)
   1. [Hands-On Swift 5 Microservices Development](#2de1619c-8407-4524-8968-1d76f370638b.xhtml)
3. [Dedication](#42276501-dfe8-42d2-b46c-70d352eb0538.xhtml)
4. [About Packt](#daea5031-bbdb-4a78-b612-fd00ec56e50b.xhtml)
   1. [Why subscribe?](#459572a5-bb21-4a6e-b809-01213dd963e1.xhtml)
