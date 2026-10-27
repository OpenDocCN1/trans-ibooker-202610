
## PROGNOSTICS AND HEALTH MANAGEMENT

A Practical Approach to Improving System Reliability Using Condition-Based Data

Douglas Goodman
James P. Hofmeister
Ferenc Szidarovszky

## Prognostics and Health Management

## Wiley Series in Quality & Reliability Engineering

Dr. Andre Kleyner

Series Editor

The Wiley Series in Quality & Reliability Engineering aims to provide a solid educational foundation for both practitioners and researchers in the Q&R field and to expand the reader's knowledge base to include the latest developments in this field. The series will provide a lasting and positive contribution to the teaching and practice of engineering.

The series coverage will contain, but is not exclusive to:

- Statistical methods
- Physics of failure
- Reliability modeling
- Functional safety
- Six-sigma methods
- Lead-free electronics
- Warranty analysis/management
- Risk and safety analysis

### Wiley Series in Quality & Reliability Engineering

- *Thermodynamic Degradation Science: Physics of Failure, Accelerated Testing, Fatigue and Reliability* by Alec Feinberg, October 2016
- *Design for Safety* by Louis J. Gullo, Jack Dixon, February 2018
- *Next Generation HALT and HASS: Robust Design of Electronics and Systems* by Kirk A. Gray, John J. Paschkewitz, May 2016
- *Reliability and Risk Models: Setting Reliability Requirements, 2nd Edition* by Michael Todinov, September 2015
- *Applied Reliability Engineering and Risk Analysis: Probabilistic Models and Statistical Inference* by Ilia B. Frenkel, Alex Karagrigoriou, Anatoly Lisnianski, Andre V. Kleyner, September 2013
- *Design for Reliability* by Dev G. Raheja (Editor), Louis J. Gullo (Editor), July 2012
- *Effective FMEAs: Achieving Safe, Reliable, and Economical Products and Processes Using Failure Modes and Effects Analysis* by Carl Carlson, April 2012
- *Failure Analysis: A Practical Guide for Manufacturers of Electronic Components and Systems* by Marius Bazu, Titu Bajenescu, April 2011
- *Reliability Technology: Principles and Practice of Failure Prevention in Electronic Systems* by Norman Pascoe, April 2011
- *Improving Product Reliability: Strategies and Implementation* by Mark A. Levin, Ted T. Kalal, March 2003

## Prognostics and Health Management

A Practical Approach to Improving System Reliability Using Condition-Based Data

Douglas Goodman, James P. Hofmeister and Ferenc Szidarovszky
Ridgetop Group, Inc., Arizona, USA

WILEY

This edition first published 2019

© 2019 John Wiley & Sons Ltd

All rights reserved. No part of this publication may be reproduced, stored in a retrieval system, or transmitted, in any form or by any means, electronic, mechanical, photocopying, recording or otherwise, except as permitted by law. Advice on how to obtain permission to reuse material from this title is available at http://www.wiley.com/go/permissions.

The right of Douglas Goodman, James P. Hofmeister and Ferenc Szidarovszky to be identified as the authors of this work has been asserted in accordance with law.

### Registered Offices

John Wiley & Sons, Inc., 111 River Street, Hoboken, NJ 07030, USA
John Wiley & Sons Ltd, The Atrium, Southern Gate, Chichester, West Sussex, PO19 8SQ, UK

### Editorial Office

The Atrium, Southern Gate, Chichester, West Sussex, PO19 8SQ, UK

For details of our global editorial offices, customer services, and more information about Wiley products visit us at www.wiley.com.

Wiley also publishes its books in a variety of electronic formats and by print-on-demand. Some content that appears in standard print versions of this book may not be available in other formats.

### Limit of Liability/Disclaimer of Warranty

While the publisher and authors have used their best efforts in preparing this work, they make no representations or warranties with respect to the accuracy or completeness of the contents of this work and specifically disclaim all warranties, including without limitation any implied warranties of merchantability or fitness for a particular purpose. No warranty may be created or extended by sales representatives, written sales materials or promotional statements for this work. The fact that an organization, website, or product is referred to in this work as a citation and/or potential source of further information does not mean that the publisher and authors endorse the information or services the organization, website, or product may provide or recommendations it may make. This work is sold with the understanding that the publisher is not engaged in rendering professional services. The advice and strategies contained herein may not be suitable for your situation. You should consult with a specialist where appropriate. Further, readers should be aware that websites listed in this work may have changed or disappeared between when this work was written and when it is read. Neither the publisher nor authors shall be liable for any loss of profit or any other commercial damages, including but not limited to special, incidental, consequential, or other damages.

### Library of Congress Cataloging-in-Publication Data

- **Names:** Goodman, Douglas (Industrial engineer), author. | Hofmeister, James P., author. | Szidarovszky, Ferenc, author.
- **Title:** Prognostics and health management : a practical approach to improving system reliability using condition-based data / Douglas Goodman, Chief Engineer, Ridgetop Group, Inc., James P. Hofmeister, Distinguished Engineer, Ridgetop Group, Inc., Ferenc Szidarovszky, Senior Researcher, Ridgetop Group, Inc.
- **Description:** Hoboken, NJ, USA : Wiley, [2019] | Includes bibliographical references and index.
- **Identifiers:** LCCN 2018060348 (print) | LCCN 2019000608 (ebook) | ISBN 9781119356691 (AdobePDF) | ISBN 9781119356707 (ePub) | ISBN 9781119356653 (hardcover)
- **Subjects:** LCSH: Machinery--Reliability. | Equipment health monitoring. | Machinery--Maintenance and repair--Planning. | Structural failures--Mathematical models.
- **Classification:** LCC TJ174 (ebook) | LCC TJ174 .G66 2019 (print) | DDC 621.8/16--dc23

LC record available at https://lccn.loc.gov/2018060348

Cover Design: Wiley
Cover Image: © Sergey Nivens/Shutterstock

Set in 10/12pt WarnockPro by SPi Global, Chennai, India

Printed in Great Britain by TJ International Ltd, Padstow, Cornwall

## Contents

- 1.7.3 Prognostic Distance (PD) and Convergence 45
- 1.7.4 Convergence: Figure of Merit (xₐ) 45
- 1.7.5 Other Sources of Non-ideality in FFS Data 46
- 1.8 Decisions on Cost and Benefits 47
  - 1.8.1 Product Selection 47
  - 1.8.2 Optimal Maintenance Scheduling 49
  - 1.8.3 Condition-Based Maintenance or Replacement 54
  - 1.8.4 Preventive Replacement Scheduling 55
  - 1.8.5 Model Variants and Extensions 58
- 1.9 Introduction to PHM: Summary 60
- References 60
- Further Reading 62
- **2 Approaches for Prognosis and Health Management/Monitoring (PHM) 63**
  - 2.1 Introduction to Approaches for Prognosis and Health Management/Monitoring (PHM) 63
    - 2.1.1 Model-Based Prognostic Approaches 63
    - 2.1.2 Data-Driven Prognostic Approaches 63
    - 2.1.3 Hybrid Prognostic Approaches 64
    - 2.1.4 Chapter Objectives 64
    - 2.1.5 Chapter Organization 64
  - 2.2 Model-Based Prognostics 65
    - 2.2.1 Analytical Modeling 66
    - 2.2.2 Distribution Modeling 71
    - 2.2.3 Physics of Failure (PoF) and Reliability Modeling 72
    - 2.2.4 Acceleration Factor (AF) 74
    - 2.2.5 Complexity Related to Reliability Modeling 76
    - 2.2.6 Failure Distribution 78
    - 2.2.7 Multiple Modes of Failure: Failure Rate and FIT 79
    - 2.2.8 Advantages and Disadvantages of Model-Based Prognostics 79
  - 2.3 Data-Driven Prognostics 80
    - 2.3.1 Statistical Methods 80
    - 2.3.2 Machine Learning (ML): Classification and Clustering 85
  - 2.4 Hybrid-Driven Prognostics 90
  - 2.5 An Approach to Condition-Based Maintenance (CBM) 92
    - 2.5.1 Modeling of Condition-Based Data (CBD) Signatures 92
    - 2.5.2 Comparison of Methodologies: Life Consumption and CBD Signature 92
    - 2.5.3 CBD-Signature Modeling: An Illustration 93
  - 2.6 Approaches to PHM: Summary 103
  - References 103
  - Further Reading 106
- **3 Failure Progression Signatures 107**
  - 3.1 Introduction to Failure Signatures 107
    - 3.1.1 Chapter Objectives 107
    - 3.1.2 Chapter Organization 108
