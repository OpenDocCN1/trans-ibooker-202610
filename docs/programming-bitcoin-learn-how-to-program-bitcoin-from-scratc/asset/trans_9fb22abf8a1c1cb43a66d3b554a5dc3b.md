

# 编程比特币

by Jimmy Song

Copyright © 2019 Jimmy Song. All rights reserved.

Printed in the United States of America.

Published by O'Reilly Media, Inc., 1005 Gravenstein Highway North, Sebastopol, CA 95472.

O'Reilly books may be purchased for educational, business, or sales promotional use. Online editions are also available for most titles (http://oreilly.com). For more information, contact our corporate/institutional sales department: 800-998-9938 or corporate@oreilly.com.

Editors: Mike Loukides and Michele Cronin
Production Editor: Kristen Brown
Copyeditor: James Fraleigh
Proofreader: Rachel Head

Indexer: Judy McConville
Interior Designer: David Futato
Cover Designer: Karen Montgomery
Illustrator: Rebecca Demarest

March 2019: First Edition

# Revision History for the First Edition

2019-02-08: First Release

See http://oreilly.com/catalog/errata.csp?isbn=9781492031499 for release details.

The O'Reilly logo is a registered trademark of O'Reilly Media, Inc. Programming Bitcoin, the cover image, and related trade dress are trademarks of O'Reilly Media, Inc.

The views expressed in this work are those of the author, and do not represent the publisher's views. While the publisher and the author have used good faith efforts to ensure that the information and instructions contained in this work are accurate, the publisher and the author disclaim all responsibility for errors or omissions, including without limitation responsibility for damages resulting from the use of or reliance on this work. Use of the information and instructions contained in this work is at your own risk. If any code samples or other technology this work contains or describes is subject to open source licenses or the intellectual property rights of others, it is your responsibility to ensure that your use thereof complies with such licenses and/or rights.

+   Locktime 98
+     Exercise 4 98
+     Exercise 5 98
+   Coding Transactions 99
+   Transaction Fee 100
+     Calculating the Fee 102
+     Exercise 6 102
+   Conclusion 102
+   
+   6. Script......................................................... 103
+   Mechanics of Script 103
+   How Script Works 105
+   Example Operations 105
+     Coding Opcodes 106
+     Exercise 1 107
+   Parsing the Script Fields 107
+     Coding a Script Parser and Serializer 108
+   Combining the Script Fields 111
+     Coding the Combined Instruction Set 111
+   Standard Scripts 111
+   p2pk 112
+     Coding Script Evaluation 115
+     Stack Elements Under the Hood 117
+     Exercise 2 118
+   Problems with p2pk 118
+   Solving the Problems with p2pkh 120
+   p2pkh 120
+   Scripts Can Be Arbitrarily Constructed 124
+   Exercise 3 127
+   Utility of Scripts 127
+   Exercise 4 127
+   SHA-1 Piñata 128
+   Conclusion 128
+   
+   7. Transaction Creation and Validation................................ 129
+   Validating Transactions 129
+     Checking the Spentness of Inputs 130
+     Checking the Sum of the Inputs Versus the Sum of the Outputs 130
+     Checking the Signature 131
+     Exercise 1 135
+     Exercise 2 135
+   Verifying the Entire Transaction 135

Timestamp 169

Bits 170

Nonce 170

Proof-of-Work 170

How a Miner Generates New Hashes 171

The Target 172

Exercise 9 173

Difficulty 173

Exercise 10 173

Checking That the Proof-of-Work Is Sufficient 174

Exercise 11 174

Difficulty Adjustment 174

Exercise 12 176

Exercise 13 176

Conclusion 176

# 10. Networking. 177

Network Messages 177

Exercise 1 179

Exercise 2 179

Exercise 3 179

Parsing the Payload 179

Exercise 4 181

Network Handshake 181

Connecting to the Network 181

Exercise 5 184

Getting Block Headers 184

Exercise 6 185

Headers Response 185

Conclusion 188

# 11. Simplified Payment Verification 189

Motivation 189

Merkle Tree 190

Merkle Parent 191

Exercise 1 192

Merkle Parent Level 192

Exercise 2 193

Merkle Root 193

Exercise 3 194

Merkle Root in Blocks 194

Exercise 4 195

There is no better way to understand a technology than to build something useful to you in it. Until you’ve coded the fundamental building blocks of a blockchain application with your own hands, you will not be able to intuit the difference between empty hype and realizable possibility.

This book is the most efficient and comprehensive way to learn about Bitcoin and blockchain through coding. With great skill and insight, Jimmy Song has crafted a learning path that will take you from the basic math behind Bitcoin to the latest extensions and forks. Along the way, the exercises—refined through extensive work with live students—will not only teach you the mechanics of working with the blockchain, but also an intuition for the elegance and beauty of this technology.

The journey will not be easy. Even with a teacher as adept as Jimmy to guide you, this isn’t a book to be flipped through when you’re bored from bingeing on Netflix. It requires you to put in considerable work to get the most out of it. There is no shortcut, no CliffsNotes. But that is very much in line with the constitutive principle of Bitcoin: you must have skin in the game; you must demonstrate proof-of-work. Only then can you trust your knowledge.

Happy coding!

— Ken Liu

A winner of the Nebula, Hugo, and World Fantasy awards, Ken Liu is the author of The Dandelion Dynasty, a silkpunk epic fantasy series in which the magic is engineering, and The Paper Menagerie and Other Stories, a collection. His SF story about blockchain, “Byzantine Empathy”, was originally published by the MIT Press.

# Preface

This book will teach you the technology of Bitcoin at a fundamental level. It doesn’t cover the monetary, economic, or social dynamics of Bitcoin, but knowing how Bitcoin works under the hood should give you greater insight into what’s possible. There’s a tendency to hype Bitcoin and blockchain without really understanding what’s going on; this book is meant to be an antidote to that tendency.

After all, there are lots of books about Bitcoin, covering the history and the economic aspects and giving technical descriptions. The aim of this book is to get you to understand Bitcoin by coding all of the components necessary for a Bitcoin library. The library is not meant to be exhaustive or efficient. The aim of the library is to help you learn.

# Who Is This Book For?

This book is for programmers who want to learn how Bitcoin works by coding it themselves. You will learn Bitcoin by coding the “bare metal” stuff in a Bitcoin library you will create from scratch. This is not a reference book where you can look up the specification for a particular feature.

The material from this book has been largely taken from my two-day seminar where I teach developers all about Bitcoin. The material has been refined extensively, as I’ve taught this course more than 20 times, to over 400 people as of this writing.

By the time you’re done with the book, you’ll not only be able to create transactions, but also get all the data you need from peers and send the transactions over the network. It covers everything needed to accomplish this, including the math, parsing, network connectivity, and block validation.

# What Do I Need to Know?

A prerequisite for this book is that you know programming—Python, in particular. The library itself is written in Python 3, and a lot of the exercises can be done in a controlled environment like a Jupyter notebook. An intermediate knowledge of Python is preferable, but even a beginning knowledge is probably sufficient to get a lot of the concepts.

Some knowledge of math is required, especially for Chapters 1 and 2. These chapters introduce mathematical concepts probably not familiar to those who didn’t major in mathematics. Math knowledge around algebra level should suffice to understand the new concepts and to code the exercises covered in those chapters.

General computer science knowledge, for example, of hash functions, will come in handy but is not strictly necessary to complete the exercises in this book.

# How Is the Book Arranged?

This book is split into 14 chapters. Each is meant to build on the previous one as the Bitcoin library gets built from scratch all the way to the end.

Roughly speaking, Chapters 1–4 establish the mathematical tools that we need; Chapters 5–8 cover transactions, which are the fundamental unit of Bitcoin; and Chapters 9–12 cover blocks and networking. The last two chapters cover some advanced topics but don’t actually require you to write code.

Chapters 1 and 2 cover some math that we need. Finite fields and elliptic curves are needed to understand elliptic curve cryptography in Chapter 3. After we’ve established the public key cryptography at the end of Chapter 3, Chapter 4 adds parsing and serialization, which are how cryptographic primitives are stored and transmitted.

Chapter 5 covers the transaction structure. Chapter 6 goes into the smart contract language behind Bitcoin, called Script. Chapter 7 builds on all the previous chapters, showing how to validate and create transactions based on the elliptic curve cryptography from the first four chapters. Chapter 8 establishes how pay-to-script-hash (p2sh) works, which is a way to make more powerful smart contracts.

Chapter 9 covers blocks, which are groups of ordered transactions. Chapter 10 covers network communication in Bitcoin. Chapters 11 and 12 go into how a light client, or software without access to the entire blockchain, might request and broadcast data to and from nodes that store the entire blockchain.

Chapter 13 covers Segwit, a backward-compatible upgrade introduced in 2017, and Chapter 14 provides suggestions for further study. These chapters are not strictly necessary, but are included as a way to give you a taste of what more there is to learn.

