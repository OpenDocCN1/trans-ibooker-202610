
## Fifth Edition
## LPIC-1®
## Linux Professional Institute Certification
## STUDY GUIDE
## EXAM 101-500 and EXAM 102-500

Includes interactive online learning environment and study tools:

- 2 custom practice exams
- 100 electronic flashcards
- Searchable key term glossary

CHRISTINE BRESNAHAN
RICHARD BLUM

SYBEX
A Wiley Brand

## LPIC-1®
Study Guide
Fifth Edition

![](img/ca7617340ddbe2360d36ce3428729fa9_2_0.png)

## LPIC-1®

## Linux Professional Institute Certification

## Study Guide

## Fifth Edition

![](img/ca7617340ddbe2360d36ce3428729fa9_4_0.png)

Christine Bresnahan

Richard Blum

SYBEX
A Wiley Brand

Copyright © 2020 by John Wiley & Sons, Inc., Indianapolis, Indiana
Published simultaneously in Canada
ISBN: 978-1-119-58212-0
ISBN: 978-1-119-58209-0 (ebk.)
ISBN: 978-1-119-58208-3 (ebk.)
Manufactured in the United States of America

No part of this publication may be reproduced, stored in a retrieval system or transmitted in any form or by any means, electronic, mechanical, photocopying, recording, scanning or otherwise, except as permitted under Sections 107 or 108 of the 1976 United States Copyright Act, without either the prior written permission of the Publisher, or authorization through payment of the appropriate per-copy fee to the Copyright Clearance Center, 222 Rosewood Drive, Danvers, MA 01923, (978) 750-8400, fax (978) 646-8600. Requests to the Publisher for permission should be addressed to the Permissions Department, John Wiley & Sons, Inc., 111 River Street, Hoboken, NJ 07030, (201) 748-6011, fax (201) 748-6008, or online at http://www.wiley.com/go/permissions.

Limit of Liability/Disclaimer of Warranty: The publisher and the author make no representations or warranties with respect to the accuracy or completeness of the contents of this work and specifically disclaim all warranties, including without limitation warranties of fitness for a particular purpose. No warranty may be created or extended by sales or promotional materials. The advice and strategies contained herein may not be suitable for every situation. This work is sold with the understanding that the publisher is not engaged in rendering legal, accounting, or other professional services. If professional assistance is required, the services of a competent professional person should be sought. Neither the publisher nor the author shall be liable for damages arising herefrom. The fact that an organization or Web site is referred to in this work as a citation and/or a potential source of further information does not mean that the author or the publisher endorses the information the organization or Web site may provide or recommendations it may make. Further, readers should be aware that Internet Web sites listed in this work may have changed or disappeared between when this work was written and when it is read.

For general information on our other products and services or to obtain technical support, please contact our Customer Care Department within the U.S. at (877) 762-2974, outside the U.S. at (317) 572-3993 or fax (317) 572-4002.

Wiley publishes in a variety of print and electronic formats and by print-on-demand. Some material included with standard print versions of this book may not be included in e-books or in print-on-demand. If this book refers to media such as a CD or DVD that is not included in the version you purchased, you may download this material at http://booksupport.wiley.com. For more information about Wiley products, visit www.wiley.com.

Library of Congress Control Number: 2019950102

TRADEMARKS: Wiley, the Wiley logo, and the Sybex logo are trademarks or registered trademarks of John Wiley & Sons, Inc. and/or its affiliates, in the United States and other countries, and may not be used without written permission. LPIC-1 is a registered trademark of Linux Professional Institute, Inc. All other trademarks are the property of their respective owners. John Wiley & Sons, Inc. is not associated with any product or vendor mentioned in this book.

10 9 8 7 6 5 4 3 2 1

## Acknowledgments

First, all glory and praise go to God, who through His Son, Jesus Christ, makes all things possible, and gives us the gift of eternal life.

Many thanks go to the fantastic team of people at Sybex for their outstanding work on this project. Thanks to Kenyon Brown, the senior acquisitions editor, for offering us the opportunity to work on this book. Also thanks to Stephanie Barton, the development editor, for keeping things on track and making the book more presentable. Thanks Steph, for all your hard work and diligence. The technical editor, David Clinton, did a wonderful job of double-checking all of the code in the book in addition to making suggestions to improve the content. Thanks also goes to the young and talented Daniel Anez (theanez.com) for his illustration work. We would also like to thank Carole Jelen at Waterside Productions, Inc., for arranging this opportunity for us and for helping us out in our writing careers.

Christine would particularly like to thank her husband, Timothy, for his encouragement, patience, and willingness to listen, even when he has no idea what she is talking about. Christine would also like to express her love for Samantha and Cameron, “May God bless your marriage richly.”

Rich would particularly like to thank his wife, Barbara, for enduring his grouchy attitude during this project, and helping to keep up his spirits with baked goods.

## About the Authors

Christine Bresnahan, CompTIA Linux+, started working with computers more than 30 years ago in the IT industry as a systems administrator. Christine is an adjunct professor at Ivy Tech Community College where she teaches Linux certification and Python programming classes. She also writes books and produces instructional resources for the classroom.

Richard Blum, CompTIA Linux+ ce, CompTIA Security+ ce, has also worked in the IT industry for more than 30 years as both a system and network administrator, and he has published numerous Linux and open source books. Rich is an online instructor for Linux and web programming courses that are used by colleges and universities across the United States. When he is not being a computer nerd, Rich enjoys spending time with his wife Barbara and his two daughters, Katie and Jessica.

## Contents at a Glance

| | | |
|---|---|---|
| Introduction | | xxi |
| Assessment Test | | xxxix |
| Part I | Exam 101-500 | 1 |
| Chapter 1 | Exploring Linux Command-Line Tools | 3 |
| Chapter 2 | Managing Software and Processes | 67 |
| Chapter 3 | Configuring Hardware | 133 |
| Chapter 4 | Managing Files | 181 |
| Chapter 5 | Booting, Initializing, and Virtualizing Linux | 245 |
| Part II | Exam 102-500 | 303 |
| Chapter 6 | Configuring the GUI, Localization, and Printing | 305 |
| Chapter 7 | Administering the System | 353 |
| Chapter 8 | Configuring Basic Networking | 423 |
| Chapter 9 | Writing Scripts | 465 |
| Chapter 10 | Securing Your System | 523 |
| Appendix | Answers to Review Questions | 583 |
| Index | | 619 |

## Contents

- Introduction ... xxi
- Assessment Test ... xxxix
- **Part I** ... **Exam 101-500** ... 1
- **Chapter 1** ... **Exploring Linux Command-Line Tools** ... 3
  - Understanding Command-Line Basics ... 4
  - Discussing Distributions ... 4
  - Reaching a Shell ... 5
  - Exploring Your Linux Shell Options ... 5
  - Using a Shell ... 7
  - Using Environment Variables ... 11
  - Getting Help ... 17
  - Editing Text Files ... 20
  - Looking at Text Editors ... 20
  - Understanding vim Modes ... 24
  - Exploring Basic Text-Editing Procedures ... 24
  - Saving Changes ... 27
  - Processing Text Using Filters ... 28
  - File-Combining Commands ... 28
  - File-Transforming Commands ... 31
  - File-Formatting Commands ... 33
  - File-Viewing Commands ... 36
  - File-Summarizing Commands ... 40
  - Using Regular Expressions ... 45
  - Using grep ... 45
  - Understanding Basic Regular Expressions ... 47
  - Understanding Extended Regular Expressions ... 50
  - Using Streams, Redirection, and Pipes ... 50
  - Redirecting Input and Output ... 51
  - Piping Data between Programs ... 55
  - Using sed ... 56
  - Generating Command Lines ... 60
  - Summary ... 61
  - Exam Essentials ... 61
  - Review Questions ... 62
