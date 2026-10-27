**Code reuse/dead code**  Code reuse is a common programming practice that can add to the security of the application. You would create a set of functions that follow secure coding principles and then call those functions from any of your applications that need the functionality. The purpose is that you know you are calling already tested and tried code, versus re-creating new code that could have vulnerabilities. You should also evaluate your code and look for dead code, which is code in a function that executes and returns a result, but the result is never used in the application anywhere. Dead code should be removed because the code could be vulnerable to an exploit if left unmaintained and then forgotten over time. For example, you may have an API that has a function that is no longer used. The function should be removed from the API to prevent future exploits against the function.

**Server-side vs. client-side execution and validation**  From a secure application design point of view, it is more secure to have server-side code execute than client-side code. Client-side code can be changed at the client and really should not be relied upon. For example, you should always implement validation code on the server side, but whenever possible it does not hurt to implement validation code on both the client side and the server side.

**Memory management**  It is critical that developers learn to properly manage memory. This involves freeing memory from objects that are no longer in use by properly destroying those objects. If a developer fails to clean up memory, this could cause memory leaks in the application.

**Use of third-party libraries and SDKs**  Limit the use of third-party DLLs in your application and software development kits (SDKs). You do not know how secure the code is in those components because you did not write it.

**Data exposure**  Be sure to limit the amount of data that is exposed from your application. One of the techniques to do this is known as *encapsulation*. You can create public methods that interact with the data without making the data directly available.

# Application Frameworks and Scripting

As we finish off this section, I want to address a few concepts mentioned in the Security+ objectives that are related to applications and application security.

# Open Web Application Security Project

As a security professional, if you are responsible for testing software for secure coding practices, you should definitely check out the Open Web Application Security Project (OWASP). This project is supported by an online community and has many articles, tools, and documents published that are focused on educating people on common vulnerabilities in web applications and common security techniques that should be used to protect web applications.