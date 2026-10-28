
![](img/4bb66282061cdabd9d492d7740bb0233_0_0.png)

## MODEL-BASED PROCESSING

An Applied Subspace Identification Approach

JAMES V. CANDY

WILEY

## Model-Based Processing

An Applied Subspace Identification Approach

James V. Candy
Lawrence Livermore National Laboratory
University of California, Santa Barbara

This edition first published 2019

© 2019 John Wiley & Sons, Inc.

All rights reserved. No part of this publication may be reproduced, stored in a retrieval system, or transmitted, in any form or by any means, electronic, mechanical, photocopying, recording or otherwise, except as permitted by law. Advice on how to obtain permission to reuse material from this title is available at http://www.wiley.com/go/permissions.

The right of James V. Candy to be identified as the author of this work has been asserted in accordance with law.

*Registered Office*
John Wiley & Sons, Inc., 111 River Street, Hoboken, NJ 07030, USA

*Editorial Office*
111 River Street, Hoboken, NJ 07030, USA

For details of our global editorial offices, customer services, and more information about Wiley products, visit us at www.wiley.com.

Wiley also publishes its books in a variety of electronic formats and by print-on-demand. Some content that appears in standard print versions of this book may not be available in other formats.

*Limit of Liability/Disclaimer of Warranty*
MATLAB® is a trademark of The MathWorks, Inc. and is used with permission. The MathWorks does not warrant the accuracy of the text or exercises in this book. This work’s use or discussion of MATLAB® software or related products does not constitute endorsement or sponsorship by The MathWorks of a particular pedagogical approach or particular use of the MATLAB® software. While the publisher and authors have used their best efforts in preparing this work, they make no representations or warranties with respect to the accuracy or completeness of the contents of this work and specifically disclaim all warranties, including without limitation any implied warranties of merchantability or fitness for a particular purpose. No warranty may be created or extended by sales representatives, written sales materials or promotional statements for this work. The fact that an organization, website, or product is referred to in this work as a citation and/or potential source of further information does not mean that the publisher and authors endorse the information or services the organization, website, or product may provide or recommendations it may make. This work is sold with the understanding that the publisher is not engaged in rendering professional services. The advice and strategies contained herein may not be suitable for your situation. You should consult with a specialist where appropriate. Further, readers should be aware that websites listed in this work may have changed or disappeared between when this work was written and when it is read. Neither the publisher nor authors shall be liable for any loss of profit or any other commercial damages, including but not limited to special, incidental, consequential, or other damages.

*Library of Congress Cataloging-in-Publication Data applied for*
ISBN: 9781119457763

Cover design by Wiley
Cover image: Courtesy of Patricia Candy

Set in 10/12pt WarnockPro by SPi Global, Chennai, India

Printed in the United States of America

10   9   8   7   6   5   4   3   2   1

## Contents

- Preface *xiii*
- Acknowledgements *xxi*
- Glossary *xxiii*

- **1 Introduction** *1*
  - 1.1 Background *1*
  - 1.2 Signal Estimation *2*
  - 1.3 Model-Based Processing *8*
  - 1.4 Model-Based Identification *16*
  - 1.5 Subspace Identification *20*
  - 1.6 Notation and Terminology *22*
  - 1.7 Summary *24*
  - MATLAB Notes *25*
  - References *25*
  - Problems *26*

- **2 Random Signals and Systems** *29*
  - 2.1 Introduction *29*
  - 2.2 Discrete Random Signals *32*
  - 2.3 Spectral Representation of Random Signals *36*
  - 2.4 Discrete Systems with Random Inputs *40*
    - 2.4.1 Spectral Theorems *41*
    - 2.4.2 ARMAX Modeling *42*
  - 2.5 Spectral Estimation *44*
    - 2.5.1 Classical (Nonparametric) Spectral Estimation *44*
      - 2.5.1.1 Correlation Method (Blackman–Tukey) *45*
      - 2.5.1.2 Average Periodogram Method (Welch) *46*
    - 2.5.2 Modern (Parametric) Spectral Estimation *47*
      - 2.5.2.1 Autoregressive (All-Pole) Spectral Estimation *48*
      - 2.5.2.2 Autoregressive Moving Average Spectral Estimation *51*
      - 2.5.2.3 Minimum Variance Distortionless Response (MVDR) Spectral Estimation *52*
      - 2.5.2.4 Multiple Signal Classification (MUSIC) Spectral Estimation *55*
  - 2.6 Case Study: Spectral Estimation of Bandpass Sinusoids *59*
  - 2.7 Summary *61*
  - MATLAB Notes *61*
  - References *62*
  - Problems *64*

- **3 State-Space Models for Identification** *69*
  - 3.1 Introduction *69*
  - 3.2 Continuous-Time State-Space Models *69*
  - 3.3 Sampled-Data State-Space Models *73*
  - 3.4 Discrete-Time State-Space Models *74*
    - 3.4.1 Linear Discrete Time-Invariant Systems *77*
    - 3.4.2 Discrete Systems Theory *78*
    - 3.4.3 Equivalent Linear Systems *82*
    - 3.4.4 Stable Linear Systems *83*
  - 3.5 Gauss–Markov State-Space Models *83*
    - 3.5.1 Discrete-Time Gauss–Markov Models *83*
  - 3.6 Innovations Model *89*
  - 3.7 State-Space Model Structures *90*
    - 3.7.1 Time-Series Models *91*
    - 3.7.2 State-Space and Time-Series Equivalence Models *91*
  - 3.8 Nonlinear (Approximate) Gauss–Markov State-Space Models *97*
  - 3.9 Summary *101*
  - MATLAB Notes *102*
  - References *102*
  - Problems *103*

- **4 Model-Based Processors** *107*
  - 4.1 Introduction *107*
  - 4.2 Linear Model-Based Processor: Kalman Filter *108*
    - 4.2.1 Innovations Approach *110*
    - 4.2.2 Bayesian Approach *114*
    - 4.2.3 Innovations Sequence *116*
    - 4.2.4 Practical Linear Kalman Filter Design: Performance Analysis *117*
    - 4.2.5 Steady-State Kalman Filter *125*
    - 4.2.6 Kalman Filter/Wiener Filter Equivalence *128*
  - 4.3 Nonlinear State-Space Model-Based Processors *129*
    - 4.3.1 Nonlinear Model-Based Processor: Linearized Kalman Filter *130*
    - 4.3.2 Nonlinear Model-Based Processor: Extended Kalman Filter *133*
    - 4.3.5 Practical Nonlinear Model-Based Processor Design: Performance Analysis *148*
    - 4.3.6 Nonlinear Model-Based Processor: Particle Filter *151*
    - 4.3.7 Practical Bayesian Model-Based Design: Performance Analysis *160*
  - 4.4 Case Study: 2D-Tracking Problem *166*
  - 4.5 Summary *173*
  - MATLAB Notes *173*
  - References *174*
  - Problems *177*

- **5 Parametrically Adaptive Processors** *185*
  - 5.1 Introduction *185*
  - 5.2 Parametrically Adaptive Processors: Bayesian Approach *186*
  - 5.3 Parametrically Adaptive Processors: Nonlinear Kalman Filters *187*
    - 5.3.1 Parametric Models *188*
    - 5.3.2 Classical Joint State/Parametric Processors: Augmented Extended Kalman Filter *190*
    - 5.3.3 Modern Joint State/Parametric Processor: Augmented Unscented Kalman Filter *198*
  - 5.4 Parametrically Adaptive Processors: Particle Filter *201*
    - 5.4.1 Joint State/Parameter Estimation: Particle Filter *201*
  - 5.5 Parametrically Adaptive Processors: Linear Kalman Filter *208*
  - 5.6 Case Study: Random Target Tracking *214*
  - 5.7 Summary *222*
  - MATLAB Notes *223*
  - References *223*
  - Problems *226*

- **6 Deterministic Subspace Identification** *231*
  - 6.1 Introduction *231*
  - 6.2 Deterministic Realization Problem *232*
    - 6.2.1 Realization Theory *233*
    - 6.2.2 Balanced Realizations *238*
    - 6.2.3 Systems Theory Summary *239*
  - 6.3 Classical Realization *241*
    - 6.3.1 Ho–Kalman Realization Algorithm *241*
  - 6.4.2 Multivariable Output Error State-Space (MOESP) Algorithm *271*
  - 6.5 Deterministic Subspace Realization: Oblique Projections *274*
    - 6.5.1 Subspace Realization: Oblique Projections *278*
    - 6.5.2 Numerical Algorithms for Subspace State-Space System Identification (N4SID) Algorithm *280*
  - 6.6 Model Order Estimation and Validation *285*
    - 6.6.1 Order Estimation: SVD Approach *286*
    - 6.6.2 Model Validation *289*
  - 6.7 Case Study: Structural Vibration Response *295*
  - 6.8 Summary *299*
  - MATLAB Notes *300*
  - References *300*
  - Problems *303*

- **7 Stochastic Subspace Identification** *309*
  - 7.1 Introduction *309*
  - 7.2 Stochastic Realization Problem *312*
    - 7.2.1 Correlated Gauss–Markov Model *312*
    - 7.2.2 Gauss–Markov Power Spectrum *313*
    - 7.2.3 Gauss–Markov Measurement Covariance *314*
    - 7.2.4 Stochastic Realization Theory *315*
  - 7.3 Classical Stochastic Realization via the Riccati Equation *317*
  - 7.4 Classical Stochastic Realization via Kalman Filter *321*
    - 7.4.1 Innovations Model *321*
    - 7.4.2 Innovations Power Spectrum *322*
    - 7.4.3 Innovations Measurement Covariance *323*
    - 7.4.4 Stochastic Realization: Innovations Model *325*
  - 7.5 Stochastic Subspace Realization: Orthogonal Projections *330*
    - 7.5.1 Multivariable Output Error State-Space (MOESP) Algorithm *334*
  - 7.6 Stochastic Subspace Realization: Oblique Projections *342*
    - 7.6.1 Numerical Algorithms for Subspace State-Space System Identification (N4SID) Algorithm *346*
    - 7.6.2 Relationship: Oblique (N4SID) and Orthogonal (MOESP) Algorithms *351*
  - 7.7 Model Order Estimation and Validation *353*
    - 7.7.1 Order Estimation: Stochastic Realization Problem *354*
      - 7.7.1.1 Order Estimation: Statistical Methods *356*
    - 7.7.2 Model Validation *362*
      - 7.7.2.1 Residual Testing *363*
  - 7.8 Case Study: Vibration Response of a Cylinder: Identification and Tracking *369*
  - 7.9 Summary *378*
  - MATLAB Notes *378*
  - References *379*
  - Problems *382*
