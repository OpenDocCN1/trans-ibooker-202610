request to the appropriate application or service on that system. In our example, we will assume that you are sitting at Computer1 and you have typed the address of Server1 into your web browser to create an HTTP request.

There are many, many examples of protocols that run at the application layer. The most common are HTTP, telnet, Secure Shell (SSH), File Transfer Protocol (FTP), Post Office Protocol 3 (POP3), and Simple Mail Transfer Protocol (SMTP).

![](bbox=[0.14, 0.31, 0.24, 0.37])

EXAM TIP For the exams, remember that the top (seventh) layer of the OSI Reference Model, the application layer, provides the protocols and services that enable systems to initiate a request (sending system) or service a request (receiving system). Examples of application layer protocols include telnet, FTP, HTTP (for web browsing), SSH, POP3, and SMTP.

# Layer 6: The Presentation Layer

After the request is made, the application layer passes the data down to the presentation layer, where it is formatted so that the data (or request) can be interpreted by the receiving system. When the presentation layer receives data from the application layer, it makes sure the data is in the proper format—if it is not, the presentation layer converts the data accordingly. On the receiving system, when the presentation layer receives network data from the session layer, it makes sure the data is in the proper format and once again converts it if it is not.

Formatting functions that could occur at the sending system’s presentation layer include compression, encryption, and ensuring that the character code set can be interpreted on the other side. For example, if we choose to compress our data from the application that we are using, the application layer will pass that request to the presentation layer, but the presentation layer does the compression. At the receiving end, this data