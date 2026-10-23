
![](img/879ce8537ecb7481ad77093e6fcbe574_0_0.png)

## Innovative SAP SuccessFactors Recruiting

A Guide to Creating Custom Integration and Automation

- Anand ‘Andy’ Athanur
- Mark Ingram
- Michael A. Wellens

Apress®

## Innovative SAP SuccessFactors Recruiting: A Guide to Creating Custom Integration and Automation

Anand ‘Andy’ Athanur  
San Francisco, CA, USA

Mark Ingram  
Edmonds, WA, USA

Michael A. Wellens  
Davidson, NC, USA

ISBN-13 (pbk): 978-1-4842-7424-8

ISBN-13 (electronic): 978-1-4842-7425-5

https://doi.org/10.1007/978-1-4842-7425-5

Copyright © 2022 by Anand ‘Andy’ Athanur, Mark Ingram and Michael A. Wellens

This work is subject to copyright. All rights are reserved by the Publisher, whether the whole or part of the material is concerned, specifically the rights of translation, reprinting, reuse of illustrations, recitation, broadcasting, reproduction on microfilms or in any other physical way, and transmission or information storage and retrieval, electronic adaptation, computer software, or by similar or dissimilar methodology now known or hereafter developed.

Trademarked names, logos, and images may appear in this book. Rather than use a trademark symbol with every occurrence of a trademarked name, logo, or image we use the names, logos, and images only in an editorial fashion and to the benefit of the trademark owner, with no intention of infringement of the trademark.

The use in this publication of trade names, trademarks, service marks, and similar terms, even if they are not identified as such, is not to be taken as an expression of opinion as to whether or not they are subject to proprietary rights.

While the advice and information in this book are believed to be true and accurate at the date of publication, neither the authors nor the editors nor the publisher can accept any legal responsibility for any errors or omissions that may be made. The publisher makes no warranty, express or implied, with respect to the material contained herein.

- Managing Director, Apress Media LLC: Welmoed Spahr
- Acquisitions Editor: Divya Modi
- Development Editor: Mark Powers
- Coordinating Editor: Divya Modi

Cover designed by eStudioCalamar

Cover image designed by Freepik (www.freepik.com)

Distributed to the book trade worldwide by Springer Science+Business Media New York, 1 New York Plaza, New York, NY 10004. Phone 1-800-SPRINGER, fax (201) 348-4505, e-mail orders-ny@springer-sbm.com, or visit www.springeronline.com. Apress Media, LLC is a California LLC and the sole member (owner) is Springer Science + Business Media Finance Inc (SSBM Finance Inc). SSBM Finance Inc is a Delaware corporation.

For information on translations, please e-mail booktranslations@springernature.com; for reprint, paperback, or audio rights, please e-mail bookpermissions@springernature.com.

Apress titles may be purchased in bulk for academic, corporate, or promotional use. eBook versions and licenses are also available for most titles. For more information, reference our Print and eBook Bulk Sales web page at http://www.apress.com/bulk-sales.

Any source code or other supplementary material referenced by the author in this book is available to readers on GitHub via the book's product page, located at www.apress.com/978-1-4842-7424-8. For more detailed information, please visit http://www.apress.com/source-code.

Printed on acid-free paper

*To Kim Lessley.*

## Table of Contents

- Process and Data Flow — 105
- Case Study End Results — 128
- Conclusions — 129
- Chapter 6: Robotic Process Automation — 131
  - Introduction — 131
  - Vendor Choices — 132
  - Relevant Business Scenarios — 132
  - Pros and Cons of Employing RPA — 132
  - When to Use RPAs for Recruiting? — 134
  - Use Cases for Robotic Process Automation in Recruiting — 135
  - Security Considerations — 150
  - Conclusions — 152
- Chapter 7: Automation Using Integration Center — 153
  - Relevant Business Scenarios — 154
  - Mass Updates of Recruiting Data Using Separate Integration Center Jobs — 155
  - Using the Integration Center to Automate Recruiting Using SuccessFactors as the Source and Destination System — 156
  - Why a SuccessFactors to SuccessFactors Integration — 156
  - Overview of SuccessFactors to SuccessFactors Integrations — 157
  - Case Study: Advancing Candidates Based on Attributes — 164
  - Technical Solution — 164
  - Conclusion — 174
- Chapter 8: Assessment Integration Framework — 177
  - Step 1: Provisioning – Check Provisioning Settings — 177
  - Step 2: Provisioning – Enable Assessment Integration Feature — 178
  - Step 3: Provisioning – Optional Configuration of hardstopStatus — 179
  - Step 4: Admin Center – Permissions for Assessment Integration — 179
  - Step 5: Admin Center – Importing/Exporting the Assessment Vendor File — 180
  - Step 6: Provisioning – Upload Vendor Assessment Packages — 181
  - Step 7: Admin Center – Setting Up Event Notification Subscription — 183
  - Step 8: Admin Center – Set Up Email Template to Associate with Assessment — 185
  - Step 9: Recruiting – Creating a Job Requisition with an Assessment and hardstopStatus — 187
  - Step 10: Recruiting – Posting the Job Requisition — 188
  - Step 11: Candidate – Undergoing an Assessment — 188
  - Step 12: Candidate – View Email Notifications — 189
  - Step 13: Assessment Order – Request and Response — 189
  - Step 14: Recruiting – Viewing Successful Assessments — 192
  - Step 15: Admin Center – Troubleshooting Your Integration — 193
  - Step 16: ODATA API Calls — 194
    - Querying JobApplicationAssessmentOrder — 194
    - Upsert #1: To Indicate That the Assessment Is in Progress — 199
    - Upsert #2: To Indicate That the Assessment Is Completed — 201
  - Step 17: Recruiting – Visual Confirmation of Assessment Report — 205
  - Conclusion — 206
- Chapter 9: Additional Custom ODATA Integrations — 207
  - Use of Open Standards — 207
  - Prebuilt Interfaces to Third-Party Solutions — 208
  - ODATA API Overview — 210
  - Case Study: Matching Applications to Requisitions Using AI — 214
  - Conclusions — 233
- Chapter 10: Background Check Integration — 235
  - Differences Between Option 1 (APPUI) and Option 2 (IS) — 236
  - Step 1: Permissions in Admin Tools – Enable Background Check Integration — 239
  - Step 2: Create Picklists for Background Check — 240
  - Step 3: Provisioning – Update the Job Requisition Template — 242
  - Step 4: Admin Tools – Enable Other Background Check Vendor — 244
  - Step 5: Which Tool Should I Choose – Integration Center or Intelligent Services? — 244
  - Step 6: Which ICD to Use – Option 1 (APPUI) or Option 2 (IS)? — 245
  - Step 7: Setting Up the Integration Center Template (ICD) for Option 1 — 245
