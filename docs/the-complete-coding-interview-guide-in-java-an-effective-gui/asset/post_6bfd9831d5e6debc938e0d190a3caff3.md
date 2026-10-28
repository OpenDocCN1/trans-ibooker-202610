
Is it possible to have only three people? If one of the fathers is also a grandfather, this means that the other father is a son and a father at the same time. This way, we have two fathers and two sons via three people. They eat three eggs and each of them has an egg. Problem solved!

As you can see, the solution is the result of a cascade of reasoning that eliminates the wrong solutions one by one. Trying to solve the problem by eliminating the wrong solutions via logical deduction is one of the approaches to solving these kinds of problems. Other problems are just about computing. Most of the time, there are no complicated computations or a lot of computations, but they require mathematical knowledge and/or deductions.

It is quite hard to claim that there are some tricks and tips that will help you solve math and logical puzzle problems in seconds. The best approach is to practice as much as possible. With that, let's proceed with the Coding challenges section.

## Coding challenges

In the following 15 coding challenges, we will focus on the most popular problems from the math and logical puzzles category. Let's get started!

### Coding challenge 1 – FizzBuzz

Adobe, Microsoft

Problem: Consider you've been given a positive integer, n. Write a problem that prints the numbers from 1 to n. For multiples of five, print fizz, for multiples of seven, print buzz, and for multiples of five and seven, print fizzbuzz. Print a new line after each string or number.

Solution: This is a simple problem that relies on your knowledge of division and the Java modulus (%) operator. When we divide two numbers, the dividend and the divisor, we get a quotient and the remainder. In Java, we can obtain the remainder of a division via the modulus (%) operator. In other words, if X is the dividend and Y is the divisor, then X modulus Y (written in Java as X % Y) returns the remainder of dividing X by Y. For example, 11(dividend) / 2(divisor) = 5(quotient) 1(remainder), so 11 % 2 = 1.

Put another way, if the remainder is 0, then the dividend is a multiple of the divisor; otherwise, it's not. So, a multiple of five must respect that X % 5 = 0, while a multiple of seven must respect that X % 7 = 0. Based on these relations, we can write the solution to this problem as follows:

```
public static void print(int n) {
	for (int i = 1; i <= n; i++) {
		if ((i % 5) == 0 && (i % 7) == 0) { // multiple of 5&7
			System.out.println("fizzbuzz");
		} else if ((i % 5) == 0) { // multiple of 5
			System.out.println("fizz");
		} else if ((i % 7) == 0) { // multiple of 7
			System.out.println("buzz");
		} else {
			System.out.println(i); // not a multiple of 5 or 7
		}
	}
}
```

The complete application is called FizzBuzz.

### Coding challenge 2 – Roman numerals

Amazon, Google, Adobe, Microsoft, Flipkart

Problem: Consider you've been given a positive integer, n. Write a snippet of code that converts this number into its Roman number representation. For example, if n=34, then the Roman number is XXXIV. You've been given the following constants, which contain the Roman number symbols:

| SYMBOL | VALUE |
|--------|-------|
| I | 1 |
| IV | 4 |
| V | 5 |
| IX | 9 |
| X | 10 |
| XL | 40 |
| L | 50 |
| XC | 90 |
| C | 100 |
| CD | 400 |
| D | 500 |
| CM | 900 |
| M | 1000 |

Figure 15.1 – Roman numbers

**Solution**: This problem relies on the fact that Roman numbers are common knowledge. If you've never heard about Roman numbers, then it is advisable to mention this to the interviewer. They will probably agree to give you another coding challenge in place of this one. But if you do know what the Roman numbers are, then great – let's see how we can write an application that solves this problem.

The algorithm for this problem can be deduced from several examples. Let's look at several use cases:

+   - *n* = 73 = 50+10+10+1+1+1 = L+X+X+I+I+I = LXXIII
+   - *n* = 558 = 500+50+5+1+1+1 = D+L+V+I+I+I = DLVIII
+   - *n* = 145 = 100+(50-10)+5 = C+(L-X)+V = C+XL+V = CXLV
+   - *n* = 34 = 10+10+10+(5-1) = X+X+X+(V-I) = X+X+X+IV = XXXIV
+   - *n* = 49 = (50-10)+(10-1) = (L-X)+(X-I) = XL+IX = XLIX

Roughly, we take the given number and try to find the Roman symbols corresponding to ones, tens, hundreds, or thousands. This algorithm can be expressed as follows:

+   1. Start from the thousand's place and print the corresponding Roman number. For example, if the digit at thousand's place is 4, then print the Roman equivalent of 4,000, which is MMMM.
+   2. Continue by dividing the number using digits at hundred's place and print the corresponding Roman number.
+   3. Continue by dividing the number using digits at ten's place and print the corresponding Roman number.
+   4. Continue by dividing the number using digits at one's place and print the corresponding Roman number.

In terms of code, this algorithm works as follows:

```
private static final String HUNDREDTHS[]
= {"", "C", "CC", "CCC", "CD", "D",
    "DC", "DCC", "DCCC", "CM"};
private static final String TENS[]
= {"", "X", "XX", "XXX",
    "XL", "L", "LX", "LXX", "LXXX", "XC"};
private static final String ONES[]
= {"", "I", "II", "III", "IV", "V",
    "VI", "VII", "VIII", "IX"};

public static String convert(int n) {
    String roman = "";
    // Step 1
    while (n >= 1000) {
        roman = roman + 'M';
        n -= 1000;
    }
    // Step 2
    roman = roman + HUNDREDTHS[n / 100];
    n = n % 100;
    // Step 3
    roman = roman + TENS[n / 10];
    n = n % 10;
    // Step 4
    roman = roman + ONES[n];
```

```
return roman;
}
```

The complete application is called RomanNumbers. Another approach relies on successive subtractions instead of division. The RomanNumbers application contains this implementation as well.

### Coding challenge 3 – Visiting and toggling 100 doors

Adobe, Microsoft, Flipkart

Problem: Consider you've been given 100 doors in a row that are initially closed. You have to visit these doors 100 times and each time you do, you start from the first door. For each visited door, you toggle it (if it is closed, then you open it, and vice versa). At the first visit, you visit all 100 doors. At the second visit, you visit every second door (#2, #4, #6 ...). At the third visit, you visit every third door (#3, #6, #9, ...). You follow this pattern until you visit only the 100th door. Write a snippet of code that reveals the state of the doors (closed or opened) after 100 visits.

Solution: The solution to this problem can be intuited by traversing several steps. At the initial state, all 100 doors are closed (in the following image, each 0 is a closed door and each 1 is an opened door):

0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000

Figure 15.2 – All the doors are closed (initial state)

Now, let's see what we can observe and conclude at each of the following steps:

At the first pass, we open every door (we visit each door, #1, #2, #3, #4, ..., #100):

1111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111

Figure 15.3 – All the doors are opened (step 1)

At the second pass, we only visit the even doors (#2, #4, #6, #8, #10, #12 ...), so the even doors are closed and the odd ones are opened:

1010101010101010101010101010101010101010101010101010101010101010101010101010101010101010101010101010

Figure 15.4 – The even doors are closed and the odd ones are opened (step 2)

Check out the steps highlighted in the preceding image. The state of door #12 has changed at steps 1, 2, 3, 4, 6, and 12. All these steps are divisors of 12. Moreover, step 1 opens the door, step 2 closes the door, step 3 opens the door, step 4 closes the door, step 6 opens the door, and step 12 closes the door. Starting from this observation, we can conclude that for every pair of divisors, the door will just end up back in its initial state, which is closed. In other words, each door that has an even number of divisors remains closed in the end.

Let's see whether this is true for a perfect square, such as 9. The reason for choosing a perfect square relies on the fact that a perfect square always has an odd number of positive divisors. For example, the divisors of 9 are 1, 3, and 9. This means that door #9 remains open.

Based on these two paragraphs, we can conclude that, after 100 visits, the doors that remain opened are those that are perfect squares (#1, #4, #9, #16, ..., #100), while the rest of the doors remain closed.

```
```java
private static final int DOORS = 100;
public static int[] visitToggle() {
	// 0 - closed door
	// 1 - opened door
	int[] doors = new int[DOORS];
	for (int i = 0; i <= (DOORS - 1); i++) {
		doors[i] = 0;
	}
	for (int i = 0; i <= (DOORS - 1); i++) {
		for (int j = 0; j <= (DOORS - 1); j++) {
			if ((j + 1) % (i + 1) == 0) {
				if (doors[j] == 0) {
					doors[j] = 1;
				} else {
					doors[j] = 0;
				}
			}
		}
	}
	return doors;
}
```
```
