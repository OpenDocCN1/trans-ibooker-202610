
macOS doesn’t come with Python 3 by default. To install it, go to https://www.python.org/ and click **Downloads**. This should offer you the option of downloading the latest version of Python for macOS. Click the link to download Python and then run the installer.

## Linux

Linux comes with Python 3, but it may be an older version of Python 3. Installation instructions will vary depending on which Linux distribution you’re using, but you should be able to install the newest version of Python using your favorite package manager.

## How to Read This Book

Reading this book from cover to cover in one sitting is likely to teach you very little. It would be like trying to learn piano by inviting someone into your house to play piano for a few hours and then kicking them out, lowering the lights, and serenading. That’s not how a practice-based skill is learned.

Here is my advice for making your way through the book:

**Space out your work.** Massing your practice into a small number of sessions is far less effective than spacing your practice out. When you feel tired, take a break. No one can tell you how much time to work before taking a break. No one can tell you how long it should take you to finish the book. It’s up to your own mind and body.

**Pause to test your understanding.** Reading about something can give us the illusion that we understand it better than we do. Applying the material forces what we know and what we think we know into alignment. For that reason, at key points in each chapter, I’ve included multiple-choice “concept check” questions that ask you to make a prediction. Take these seriously! Read each question and commit to a response without checking anything using your computer. Then, read my answer and explanation. This is an opportunity to confirm that you’re on the right track. If you answer incorrectly or answer correctly but for the wrong reason, take time to remedy your understanding before continuing. This could involve playing around a little more with the relevant Python feature being discussed, rereading material from the book, or searching for additional explanations and examples online.

**Practice programming.** Making predictions while you read will help solidify your understanding of key concepts. But you need more than that to become an adept problem-solver and programmer. You need to practice using Python to solve new problems, whose solutions you haven’t read about in the book. Each chapter concludes with a list of practice exercises. Please work through as many of these exercises as you can.

Learning how to program takes time. Don’t be discouraged if you progress slowly or make a lot of mistakes. Don’t be intimidated by any blustering peacocks that you might encounter online. Surround yourself with people and resources that can help you learn.

## Using Programming Judges

I’ve decided to structure this book around problems from *programming judge* websites. A programming judge website offers a repository of programming problems that can be solved by programmers around the world. You submit your solution—your Python code—and the site runs tests on your code. If your code produces the correct answer for each test case, then it’s likely that your solution is correct. If instead your code produces the wrong answer for one or more test cases, then your code is incorrect, and revisions are required.

There are several reasons why I think programming judges are particularly suitable sites for learning programming:

**Rapid feedback** Rapid, targeted feedback is crucial in the early stages of learning to program. Programming judges provide feedback as soon as you submit your code.

**High-quality problems** I find the problems on programming judges to be high quality. Many problems are originally from competitive programming contests. Other problems are written by individuals who are associated with a programming judge or simply want to help others learn. See the Problem Credits appendix for the source of each problem that we’ll study.

**Quantity of problems** The programming judges contain hundreds of problems. I’ve selected only a small number for this book. If you need more practice, trust me: the programming judges can provide it.

**Community features** The programming judges enable users to read and respond to comments. If you’re stuck on a problem, browse the comments for hints dropped there by others. If you’re still stuck, consider posting a comment of your own asking for help. Once you’ve successfully solved a problem, your learning is not done! Many programming judges allow you to view the code that others have submitted. Dig through a few such submissions to see how they compare to yours. There are always multiple ways to solve a problem. Perhaps your way is most intuitive to you right now, but opening yourself to other possibilities is an important step toward programming mastery.

## Making Your Programming Judge Accounts

We’ll use several programming judge websites throughout the book. That’s because each programming judge hosts some problems that are not found on other programming judges; we need multiple programming judges to cover all of the problems that I’ve chosen.

Here are the programming judges that we’ll use:

| Judge | URL |
|---|---|
| DMOJ | *https://dmoj.ca/* |
| Timus | *https://acm.timus.ru/* |
| USACO | *http://usaco.org/* |

Each programming judge requires that you create an account before you can submit code. Let’s go through the process of creating your accounts now and learn a little about the judges while we’re at it.

### The DMOJ Judge

The DMOJ judge is the judge we’ll use most often in the book. More than for any other judge, it’s worth your time exploring the DMOJ website and learning about what the judge offers.

To create an account on the DMOJ judge, go to *https://dmoj.ca/* and click **Sign up**. On the registration page that comes up, enter your username, password, and email address. This page also allows you to set your default programming language. We’ll exclusively be using the Python programming language in this book, so I suggest clicking **Python 3** here. Click **Register!** to create your account. Once registered, you can use your username and password to log in to DMOJ.

Each problem in the book begins by indicating the judge website where the problem can be found and the problem code that you should use to access it. For example, the first problem that we’ll work on in Chapter 1 is found on DMOJ and is problem **dmopc15c7p2**. To find this problem on DMOJ, click **Problems**, type **dmopc15c7p2** in the search box, and click **Go**. You should see the problem as the only result. If you click the problem title, you should see the problem itself.

When you’re ready to submit Python code for a problem, find the problem and click **Submit solution**. On the resulting page, paste your Python code into the text box and click **Submit!**. Your code will then be judged and the results shown.

### The Timus Judge

To create an account on the Timus judge, go to *https://acm.timus.ru/* and click **Register**. On the registration page that comes up, enter your name, password, email address, and other requested information. Click **Register** to create your account. Then, check your email for a message from Timus containing your judge ID. You’ll need your judge ID whenever you submit Python code.

There’s currently no way to set your default programming language, so be sure to choose the available version of Python 3 whenever you submit Python code.

We use the Timus judge only once, in Chapter 6, so I won’t say any more about it here.

### The USACO Judge

To create an account on the USACO judge, go to *http://usaco.org/* and click **Register for New Account**. On the registration page that comes up, enter your username, email address, and other requested information. Click **Submit** to create your account. Then, check your email for a
