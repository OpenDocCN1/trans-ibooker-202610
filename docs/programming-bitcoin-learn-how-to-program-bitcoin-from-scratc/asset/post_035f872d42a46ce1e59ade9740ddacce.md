
From here, you can navigate to the chapter directories. To do the exercises from Chapter 1, you would navigate to `code-ch01` (Figure P-2).

![](img/6c915536bcfd43e412f7adef942edf23_18_0.png)

Figure P-2. Jupyter directory view

From here you can open `Chapter1.ipynb` (Figure P-3).

![](img/6c915536bcfd43e412f7adef942edf23_18_1.png)

Figure P-3. Jupyter notebook

You may want to familiarize yourself with this interface if you haven’t seen it before, but the gist of Jupyter is that it can run Python code from the browser in a way that makes experimenting easy. You can run each “cell” and see the results as if this were an interactive Python shell.

A large portion of the exercises will be coding concepts introduced in the book. The unit tests are written for you, but you will need to write the Python code to make the tests pass. You can check whether your code is correct directly in Jupyter. You will need to edit the corresponding file by clicking through a link like the “this test” link in Figure P-3. This will take you to a browser tab like the one shown in Figure P-4.

![](img/6c915536bcfd43e412f7adef942edf23_19_0.png)

Figure P-4. `ecc.py`

To make the test pass, edit the file here and save.

## Answers

All the answers to the various exercises in this book are in Appendix A. They are also available in the `code-ch<xx>/answers.py` files, where `<xx>` is the chapter that you’re on.

## Conventions Used in This Book

The following typographical conventions are used in this book:

- *Italic* — Indicates new terms, URLs, email addresses, filenames, and file extensions.

## Acknowledgments

The number of people that got me here is legion. The experiences and knowledge that an author draws from tend to have a lot of sources, and it’s difficult to give them all their proper credit. This is my way of saying that I’m probably going to forget a significant number of people, and for that I sincerely apologize.

First, I want to thank my Lord Jesus Christ for putting me on this journey. If not for my faith, I would not have had the ethical conviction to blog about the importance of sound money in general and Bitcoin in particular, which ultimately led to the writing of this book.

My parents, Kathy and Kyung-Sup, had the courage to immigrate to America back when I was eight years old, which ultimately led to the opportunities I’ve had. My dad got me my first computers (a Commodore 16, a generic Hyundai 8086, and a 486 33-Mhz from a manufacturer I don’t remember), and my mom got me private programming tutoring as a sixth- and seventh-grader from a woman whose name I can’t remember. How she found her when I showed some proclivity for programming, I don’t know. That teacher-whose-name-I-can’t-remember stoked that natural tendency, and I hope this acknowledgment finds its way to her.

Speaking of which, I’ve had many teachers along the way, some of whom I hated at the time. Mr. Marain, Mrs. Edelman, and Mrs. Nelson taught me math and computer science back in high school. I’m not necessarily a fan of the current education system, but what I learned in those classes was instrumental to my love of math and programming.

My high school classmate Eric Silberstein gave me my first job out of college as a programmer at Idiom Technologies. I was slated to go down the consulting path before a fateful phone call back in 1998 that led me down the programmer/startup path instead. In a sense, I’ve never left.

At that first job, I met Ken Liu, and I’m amazed that we’re where we are in our careers writing books. He not only gave me great advice about book publishing, but also wrote the wonderful foreword. He’s also a great sounding board and an amazing friend, and I’m privileged to know him.

With regard to Bitcoin, the anonymous developer Satoshi Nakamoto invented what I thought before was impossible: decentralized digital scarcity. Bitcoin is a profound invention that the world hasn’t fully grokked. The website Slashdot introduced me to Bitcoin in 2011, and Alex Mizrahi gave me my first work as a Bitcoin developer in 2013. I had no idea what I was doing, but I learned quite a bit about what Bitcoin was about through his direction.

The Austin Bitcoin Meetup in 2013 was where I met a lot of Bitcoiners, and at the subsequent Texas Bitcoin Conference in 2014 I met a ton of people that I still know today. Though this meetup and conference are not ones I attend anymore, I am grateful for the people I met there—among them Michael Goldstein, Daniel Krawisz, and Napoleon Cole.

Alan Reiner recruited me to Armory back in 2014, and I’m grateful I had the opportunity to contribute to such a significant project.

Paxos/itBit was where I worked next. Chad Cascarilla and Raj Nair were the CEO and VP of Engineering who stretched me while I was there. Raj, in particular, made me write some blog posts for Paxos, which I was surprised to find I enjoyed. This led to blog posts on my own site, which eventually led to my seminars, which finally led to this book.

Three colleagues I met at Paxos were particularly significant. Richard Kiss, creator of pycoin, was the impetus for my writing this book, as he proposed that we write one together. Somewhere along the way, that turned into just me writing a book, but I’m grateful to him for giving me the idea. Aaron Caswell is a great dev who has helped me out in my seminars and reviewed this book. He’s a great programmer and mathematician, and a pretty good karate fighter from what I hear. Michael Flaxman has reviewed almost everything Bitcoin-related I’ve written, including my blog, many of my GitHub libraries, and this book. He’s also helped me out on my seminars and is just a great person to talk to. He’s one of those people who make others sharper, and I’m grateful for his significant friendship.

Vortex, Thomas Hunt, and Tone Vays brought me into the World Crypto Network back in 2017, which launched my YouTube career. Tone in particular has been an inspiration on working hard and being dedicated to a craft.

John Newbery was very helpful to me when I first contributed to Bitcoin Core, and he’s just a really good person, period. He’s become such a significant contributor in a relatively short time, which speaks to his talent and dedication. I’m grateful to other Core developers too, like Marco Falke, Wladimir van der Laan, Alex Morcos, Pieter Wuille, Matt Corallo, Suhas Daftuar, and Greg Maxwell, who have reviewed some of my code and blog posts.

David Harding was exceptional in his technical review of this book. He reviewed the text three times, with a lot of great comments. He’d better write a book about Bitcoin at some point, because he has an encyclopedic knowledge of nearly everything that’s happened in Bitcoin’s history.

Jim Calvin helped me get in contact with people at O’Reilly, and Mike Loukides was the intake editor who green-lighted the project. Andreas Antonopolous gave me some great pointers and recommended me to people at O’Reilly. Michele Cronin kept me on track during my year-long writing spree. Kristen Brown was the production editor who did a lot to get this book out on time. James Fraleigh did copyediting. I’m

# Defining Finite Sets

#5 means that multiplication has the same property. If $a$ is in the set, $a^{-1}$ is in the set. That is $a \cdot a^{-1} = 1$. Using the multiplicative inverse, we can define division. This will be the trickiest to define in a finite field.

If the order (or size) of the set is $p$, we can call the elements of the set, $0, 1, 2, \dots, p - 1$. These numbers are what we call the *elements* of the set, not necessarily the traditional numbers $0, 1, 2, 3$, etc. They behave in many ways like traditional numbers, but have some differences in how we add, subtract, multiply, and so forth.

In math notation the finite field set looks like this:

```text
$F_p = \{0, 1, 2, \dots, p-1\}$
```

What’s in the finite field set are the elements. $F_p$ is a specific finite field called “field of $p$” or “field of 29” or whatever the size of it is (again, the size is what mathematicians call *order*). The numbers between the {}s represent what elements are in the field. We name the elements $0, 1, 2$, etc. because these names are convenient for our purposes.

A finite field of order 11 looks like this:

```text
$F_{11} = \{0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10\}$
```

A finite field of order 17 looks like this:

```text
$F_{17} = \{0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16\}$
```

A finite field of order 983 looks like this:

```text
$F_{983} = \{0, 1, 2, \dots, 982\}$
```

Notice the order of the field is always 1 more than the largest element. You might have noticed that the field has a prime order every time. For a variety of reasons that will become clear later, it turns out that fields *must* have an order that is a power of a prime, and that the finite fields whose order is prime are the ones we’re interested in.

## Constructing a Finite Field in Python

We want to represent each finite field element, so in Python, we’ll be creating a class that represents a single finite field element. Naturally, we’ll name the class `FieldElement`.

The class represents an element in a field $F_{\text{prime}}$. The bare bones of the class look like this:
