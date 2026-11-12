Spice is platform independent and has some very nice additional features as well. They include:

+   - Spice's client side uses multiple data socket connections, and you can have multiple clients.
+   - Spice delivers desktop experience speeds similar to a local connection.
+   - Spice consumes low amounts of CPU, so you can use it with various servers that have multiple virtual machines and not adversely affect their performance.
+   - Spice allows high-quality video streaming.
+   - Spice provides live migration features, which means there are no connection interruptions if the virtual machine is being migrated to a new host.

While Spice has a single server implementation, it has several client implementations. These include remote-viewer and GNOME Boxes.

Another benefit of employing Spice is its strong security features. Transmitted data can be sent plaintext or traffic can be encrypted using TLS. Authentication between the Spice client and remote Spice server is implemented using Simple Authentication and Security Layer (SASL). This framework allows various authentication methods, as long as they are supported by SASL. Kerberos is a supported method.

If you are still dealing with X11, you can use X.Org-created Xspice to act as a stand-alone Spice server as well as an X server.

# Understanding Localization

The world is full of different languages. Not only does each country have its own language (or sometimes, sets of languages), but each country also has its own way in which people write numerical values, monetary values, and the time and date. For a Linux system to be useful in any specific location, it must adapt to the local way of doing all those things.

Localization is the ability to adapt a Linux system to a specific locale. To accomplish this, the Linux system must have a way to identify how to handle the characters contained in the local language. This section discusses just how Linux does that.

# Character Sets

At their core, computers work with ones and zeros, and Linux is no different. However, for a computer to interact with humans, it needs to know how to speak our language. This is where character sets come in.

A character set defines a standard code used to interpret and display characters in a language. Quite a few different character sets are used in the world for representing characters. The most common ones you'll run into (and the ones you'll see on the LPIC-1 exam) are as follows:

+   - ASCII: The American Standard Code for Information Interchange (ASCII) uses 7 bits to store characters found in the English language.