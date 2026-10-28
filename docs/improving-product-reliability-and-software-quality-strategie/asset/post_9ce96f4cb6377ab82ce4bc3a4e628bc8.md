
## Root Causes of Code Review Failures

![](img/50dc187750ab899846d825394ccbb93a_189_0.png)

Figure 10.4 Code review root cause Pareto.

techniques consist of the various testing methodologies also previously discussed. An additional defect prevention technique is known as defensive programming.

Defensive programming is the art of anticipating what can go wrong during the software execution and adding in code to handle those errors and exceptions. A significant percentage of any well-written software package consists of code to handle errors and unexpected behaviors. There are no readily available definitive studies on the subject, but there are estimates that as much as 80% of all code written is to handle errors and exceptions. Errors or unexpected behaviors could include incorrect user input, hardware errors, bad data, buggy code, bit errors, etc. Another class of exceptions comes from failed allocation of system resources (e.g. if the software attempts to allocate a block of memory, but no memory is available).

Potential errors should be anticipated during code design and implementation and appropriate error handling must be put in place. The potential errors or exceptions can be identified using techniques such as FMEAs, fault tree analysis (FTA), and RCA.

Each time some code interacts with a user, with external data, with another piece of code or with hardware, there is an opportunity for that external entity to behave incorrectly or unexpectedly. To the extent possible, the code should be written to be able to handle an unexpected or incorrect response. The behavior of the error-handling code can vary, depending on the circumstances. The error-handling code might be as simple as to display an error message and wait for user instruction. At the other extreme, the code might be designed to attempt to recover from the error and continue operating as intended.

Some programming languages have specific constructs that can be used for error and exception handling. In other languages, developers have to implement their own

```
// the demoTryCatchMethod() calls another demoThrowMethod() which can throw an exception

#include <iostream>
#define FrontSensorPin 10;

void demoTryCatchMethod()
{
    float pinValue = 0;

    try {
        demoThrowMethod( FrontSensorPin, pinValue );
    }
    catch( const char* msg ) {
        // code to handle the exception
        cerr << msg << endl;
    }
}

// demoThrowMethod() throws an exception if it detects an error

#define MaxPin 100;
#define MinPin 0;

void demoThrowMethod(int pin, float value)
{
    // check to see if pin has a legal value
    if (pin >= MinPin && pin <= MaxPin)
    {
        value = readPin( pin );
    }
    else // throw and error message
    {
        throw "Invalid pin number";
    }
}
```

Figure 10.5 Try-catch code example.

structures for detecting errors and handling them. In all cases, the developer must provide the code to actually do something when an error is detected.

Most modern object-oriented languages support “throw” and “try-catch” structures to make it easier for the developer to structure the error handling code. A method can “throw” an error if it detects an anomaly. The calling code can “catch” the error and handle it. This is called a try-block. The pseudocode in Figure 10.5 shows an example of the use of a try-block and throw.

Code that is intended to always recover from errors and keep functioning is known as fault tolerant. Most software does not have to be fault tolerant. Even software that must be fault tolerant does not necessarily have to provide fault tolerance in all aspects of its operation. Fault tolerance techniques include backups, redundant hardware and software, and code to reset hardware (and software) back to a known working state. Fault tolerance is not the subject of this book, so this subject will not be expounded on further.

In addition to including appropriate error-handling code, one less-obvious defensive programming technique is adequate code documentation. Most code is not worked on only once in its lifespan. Most software is used and modified for years, sometimes even decades. The code in an application may be extended or repaired by many engineers over a long period of time. Code that is well documented is easier to understand and thus is less likely to be broken when it is revised at some later date. Competent code documentation is a critical defensive programming technique to avoid future defect injection. This is important even if the original author of the code revises it at some later date. When the original engineer returns to some code that they wrote, their understanding of the code will have diminished. So, it is critical for the code flow and any method or function signatures to be adequately documented when initially written. Code reviewers should include code comment assessment as a key criteria during the reviews.

## 10.6 Effort Estimation

One might ask why a section on effort estimation is included in a book on product reliability and software quality. The answer is that if the project is not sized correctly, particularly if there is not enough time or staff allocated to deliver a product when it is needed, then quality will be sacrificed in order to make deadlines. Worse, the project might get so far behind or be so trouble ridden that the endeavor is canceled altogether. According to studies by the Standish Group [1], 31% of all software projects will be abandoned before completion and another 52% of software projects will cost nearly double their initial estimates. There are multiple reasons why the software project failure rate is so high. One of them is that the project was not sized correctly in the first place.

There are various ways to estimate the effort required to develop a software project, but they all use the same rule of thumb: count something. That is, the project estimator enumerates some criteria in the project and multiplies that count by a time factor. Some common project facets to count include estimated lines of code (LOC), number of requirements, and number of function points (FPs). These counts are then multiplied by the amount of effort historically required to implement and test previous projects of similar size. For example, if the new project is estimated to be five KLOC (kilo lines of code) of new code and historical data shows that it requires 40 person days of effort to implement and fully test 1 KLOC, then the new project would require 200 person days of effort.

On a side note, at first glance it might seem that 40 person days of effort seems like an awful lot of time to write 1000 LOC. After all, when developers are writing code, they often write hundreds of LOC per day. However, when all the work is considered that is required to produce a high-quality software product release, it is clear that actually writing the code is not the majority of time spent by a developer on the software. The other tasks, including writing requirements, designing the component, reviewing, testing, debugging, and fixing the software, all combine to take more time than just writing the code. All of these tasks need to be taken into account when determining the amount of effort required to develop a software package.

If historical effort data is not already being tracked, it is not too difficult to approximate a starting estimate. Assuming that the current software project is not the first software project implemented by an organization, then some historical data will be available.

Existing LOC can be determined by running a LOC counting tool on the current code base. The number of person days that went into developing that code can be estimated. This will allow the organization to derive a factor for person days per KLOC.

To improve future estimations, metrics for total labor and LOC should be tracked on a project-by-project basis. Over time, the accuracy of the effort estimation will improve as the input data becomes more accurate.

Another method for estimating effort for a project is the “like” method. With the like method, an engineer chooses a historical completed project that is like the new one and then uses the effort from the past project to estimate the effort for the new project. The estimate could be modified up or down, often combined with something counted. For example, an engineer might determine that new project X is like historic project Y, except that project X has 10 more inputs and outputs. Therefore, the effort for project X would be the same as the actual effort it took to do project Y plus the additional estimated effort to implement and test the additional 10 inputs and outputs.

As with all estimation, there is inherent inaccuracy in the prediction. Organizations that are determined to estimate more accurately will use multiple methods of estimation and will compare the different estimates to integrate into a combined effort estimate. Creating multiple effort estimates using different methods avoids having a single estimate that may be wildly inaccurate. As an example, a project might be sized by (i) estimating LOC and effort per LOC, (ii) estimating function points and effort per FP, and (iii) estimating by like projects. These three estimates could be averaged together to derive the effort estimate to be used for planning the project. As a general rule of thumb, unless a very high accuracy in effort estimate is required, it is probably not worth the work required to triangulate on effort estimate by using multiple different estimation techniques.

## Reference

1 The Standish Group, CHAOS Report, 2014. This report can be purchased from the Standish Group from here: https://www.standishgroup.com/store/services/chaos-report-decision-latency-theory-10-package.html.

## Further Reading

Cohn, M. (2006). Agile Estimating and Planning, 1e. Pearson Education.

Florac, W.A. and Carleton, A.D. (1999). Measuring the Software Process: Statistical Process Control for Software Process Improvement. Pearson Education.

Galorath, D.D. and Evans, M.W. (2006). Software Sizing, Estimation, and Risk Management: When Performance is Measured Performance Improves, 1e. Auerbach Publications.

Hill, P. (2010). Practical Software Project Estimation, 3e. McGraw-Hill.

Pfleeger, S.L., Wu, F., and Lewis, R. (2005). Software Cost Estimation and Sizing Methods, Issues, and Guidelines. RAND Corporation.

Stutzke, R. (2005). Estimating Software-Intensive Systems. Addison-Wesley.
