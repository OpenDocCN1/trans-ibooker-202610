+   - **Integrity**: This means that you know that data has not been altered or tampered with. We use a technique called hashing that takes the data and converts it into a numerical value called a hash or message digest. When you suspect changes have taken place, you would check the hash value against the original. If the hash value has changed, then the data has been tampered with. Common hashing algorithms covered in the exam are **Secure Hash Algorithm Version 1 (SHA1)** 160-bit and **Message Digest Version 5 (MD5)** 128-bit. SHA1 is more secure than MD5; however, MD5 is faster. The higher the number of bits, the more secure, and the lower the number, the faster it is.
+   - **Availability**: Availability ensures that data is always available; an example would be if you wanted to purchase an airplane ticket and the system came back with an error saying that you could not purchase it. This could be frustrating, and therefore, availability is important. Examples of availability include **Redundant Array of Independent Disks (RAID)**, which allows one or two disks to fail while still keeping the data available. Another example may be a fail-over cluster. In this, two servers can access the same data, and if one fails, the other can still provide the data, a data backup, or **Heating Ventilation Air Conditioning (HVAC)** that regulates the temperature for critical servers. In a datacentre, if the temperature is too hot then the servers will shut down.

# Least Privilege

**Least Privilege** is where you give someone only the most limited access required so that they can perform their job role; this is known as a *need-to-know* basis. The company will write a least privilege policy so that the administrators know how to manage it.

# Defense in Depth Model

**Defense in Depth** is the concept of protecting a company's data with a series of protective layers so that if one layer fails, another layer will already be in place to thwart an attack. We start with our data, then we encrypt it to protect it:

+   - The data is stored on a server.
+   - The data has file permissions.
+   - The data is encrypted.