
- [3] Total Order. Available online: https://en.wikipedia.org/wiki/Total_order#Strict_total_order.
- [4] std::totally_ordered, std::totally_ordered_with. Available online: https://en.cppreference.com/w/cpp/concepts/totally_ordered.
- [5] Standard Library header &lt;compare&gt;, Concepts. Available online: https://en.cppreference.com/w/cpp/utility/compare/three_way_comparable.
- [6] Standard Library header &lt;format&gt;. Available online: https://en.cppreference.com/w/cpp/header/format.
- [7] C++ compiler support. Available online: https://en.cppreference.com/w/cpp/compiler_support.

trust represents the most critical environment in which an application can be developed and tested. No developer, IT security officer, or software analyst should hack their own code. This environment is the comfort zone. Once the application leaves that comfort zone and enters the real environment, issues will start to take place. The trust boundary is hard and easy at the same time to be drawn. To create a line between the comfort zone and the real zone is not an easy task, especially if the application is running in a virtualized infrastructure, the cloud, or a big data environment.

In the comfort zone, a security threat is represented by the malicious end user. The malicious end user will aim to compromise the confidentiality and/or the integrity of the software application. One of the interesting concepts proposed is *software obfuscation*.

## Secure Coding Checklist

In this section, we will discuss and propose a secure coding checklist (it can be seen as a procedure as well). Table 7-1 shows an example of such a checklist and it can be developed as much as you want. The checklist contains minimal examples of items that can be checked when code is written in C++, no matter which operating system the code runs on. A frequent practice among developers is to suppress warnings, which is not beneficial.

In the “CERT Coding Standards and Rules” section, we will discuss the most important rules and list them in order so you can apply them to your process of developing cryptographic algorithms. Each rule is well explained within the guide.

### Table 7-1. Example of a Secure Coding Checklist

| No. # | Item to be checked | Description | Yes/No | Notes |
|---|---|---|---|---|
| 1 | *Compiler warnings* | Make sure that the compiler will output and a flag will be raised for receiving notifications of the potential errors listed for the following items:<br>- ✓ Wall<br>- ✓ Wmissing-declarations<br>- ✓ Wmissing-prototypes<br>- ✓ Wredundant-decls<br>- ✓ Wshadow<br>- ✓ Wstrict-prototypes<br>- ✓ Wformat=2<br><br>For more flags with their definitions and actions, refer to the “GCC Options to Request or Suppress Warnings” section [1]. It is very useful if complex cryptographic algorithms and security schemes are being implemented. |  |  |
| 2 | *Allocate enough memory for buffer memory when working with strings.* | Check the following functions if there is an upper limit for the destination buffer when a copy process is done until `\0` (NULL) is met. In order to avoid this situation, the recommendation is to allocate enough memory space for the destination buffer before the data is copied there.<br>- ✓ strcpy()<br>- ✓ strcat()<br>- ✓ sprintf()<br>- ✓ scanf()<br>- ✓ gets() |  |  |
| 3 | Check for direct breaks of system security. | Checking for untrusted input will lead to a direct breach of the application security. With this step, you protect the application against malicious users and attackers exploiting your program using metachars.<br>- ✓ system()<br>- ✓ popen()<br>- ✓ fork(2)<br>- ✓ exec(2)<br>- ✓ s_popen()<br>- ✓ HXproc_* [2] |  |  |
| 4 | Check for the wrong size of parameters and getting unexpected results. | When complex programs are written, such as the implementation of SHA-256 from Chapter 2, Listing 2-1, assigning a wrong size to one of the parameters or doing a wrong arithmetic operation can cause a serious pitfall and a fix should immediately be provided. Make sure that the size allocated for the parameters is the same size on the destination side. As a best practice, especially in implementing cryptography algorithms, it is better to work with size_t type. Be type-safe and don't create overflows.<br>- ✓ strncpy()<br>- ✓ strncat()<br>- ✓ snprintf() |  |  |
| 5 | **Check if too much memory is allocated.** | Allocating too much memory and external parameters represents a certain part of the size. If so, you are dealing with a wrong memory allocation and you will experience a *denial-of-service*. To avoid this from happening, it is better to follow the below criteria:<br>- `malloc(), calloc(), alloca()`<br>- No integer overflows<br>- Avoid arithmetical issues<br>- Verification for any possible operation with untrusted integer that could lead for an integer overflow. |  |  |
| 6 | **Avoid wrong casts.** | Avoid coding like below. The compiler will think that `malloc` will return an int, which is totally incorrect. It will create a bug that can easily be exploited by hackers.<br><code>char *a = malloc(10) – bad cast<br>class BaseClass {};<br>class DerivedClass: public BaseClass {};<br><br>BaseClass b; BaseClass* pb;<br>DerivedClass d; DerivedClass* pd;<br><br>//good cast<br>pb = dynamic_cast&lt;BaseClass*&gt;(&amp;d);<br><br>//bad cast<br>pd = dynamic_cast&lt;DerivedClass*&gt;(&amp;b);</code> |  |  |
| 7 | **Avoid variable parameter lists.** | When you are implementing security schemes based on strings, you may experience a new type of problem, which security analysts or ethical hackers enjoy playing with when performing tests. A simple test that is commonly used by ethical hackers to check untrusted data is to check if a function allows a variable as a list of parameters or arguments, such as `printf()`. The untrusted data (created by an ethical hacker) is directly used as a string format and not as an argument. Follow the below logic for any similar situations:<br>- Wrong way: `snprintf(buffer, sizeof(buffer), the_input_of_the_user)`<br>- Right way: `snprintf(buffer, sizeof(buffer), "%s", the_input_of_the_user)` |  |  |
| 8 | **Operations with files** | When handling files during cryptographic operations, try to use `mkstemp()`. |  |  |
| 9 | **File permissions** | Not everyone should have the ability to read or write from or to a file. In order to create files with the correct permissions, make a habit of using `umask()`.<br>- At the beginning of the file use `umask(077)`. |  |  |

## CERT Coding Standards

The CERT C++ Coding Standard was developed only for versions of the C++ programming language defined by the ISO/IEC 14882-2014 standard.

The coding standard is very well organized and it follows a certain structure: identifiers, noncompliant code examples and compliant solutions, exceptions, risk assessment, automated detection, related vulnerabilities, and related guidelines [7].

We will examine each item of the structure and we will explain the main objective and purpose.

### Identifiers

Each identifier has three parts:

- A three-letter mnemonic that represents the section within the standard
- A numeric value of two digits in the range of 00 to 99
- The language that is associated with it, which is represented as a suffix (-CPP, -C, -J, -PL)
    - -CPP: SEI CERT C++ Coding Standard [7]
    - -C: SEI CERT C Coding Standard [8]
    - -J: SEI CERT Oracle Coding Standard for Java [9]
    - -PL: SEI CERT Perl Coding Standard [10]

The three-letter mnemonic is used to group related coding practices and to point out which category a related coding belongs to.

### Noncompliant Code Examples and Compliant Solutions

The examples of noncompliant code show the code that violates the guideline. It is very important to keep in mind that they are only examples. The removing process of all appearances of the example does not mean that the code we are analyzing is compliant with the SEI CERT standard.

### Exceptions

Exceptions are of an informative character and are not required to be followed. Any of the rules can have a set of exceptions which provide details about the circumstances in which the guideline is not necessary to be followed for ensuring the safety, security, or reliability of the software.

As for any type of exception, it doesn’t matter the programming language: the principle is the same. It’s necessary to pay extra attention to the exceptions and to catch any possible exception and to learn from it. Don’t ignore them, and don’t think that a programming language is perfect and doesn’t have any bugs or certain doors that can be exploited.
