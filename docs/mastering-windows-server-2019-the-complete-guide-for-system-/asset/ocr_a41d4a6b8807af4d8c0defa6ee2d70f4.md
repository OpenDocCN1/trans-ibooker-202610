Let me be clear: I am not trying to convince you that sticking with IPv4 is the way of the future. I am just laying out the fact that, for most organizations over the next few years, this will simply be the truth. The reason I want to discuss IPv6 here is that, eventually, you will have to deal with it. And once you do, you'll actually get excited about it! There are some huge advantages that IPv6 has over IPv4, namely, the enormous number of IP addresses that you can contain within a single network. Network teams in companies around the world struggle every day with the need to build more and more IPv4 networks and tie them together. Think about it: there are many companies now with employee counts in excess of 10,000. Some have many, many times that number. In today's world, everyone needs almost constant access to their data. Data is the new currency. Most users now have at least two physical devices they utilize for work, sometimes more than that: a laptop and a tablet, or a laptop and a smartphone, or a desktop and a laptop, and a tablet and a smartphone; you get the idea. In the IPv4 world, where you are dealing with comparatively small IP address ranges, you have to get very creative with creating subnets in order to accommodate all of these physical devices. Each needs a unique IP address to communicate on the network. The biggest advantage to IPv6 is that it resolves all of these problems immediately, and by default, by providing the capability to have a huge number of IP addresses within a single network. How many more addresses are we talking about? The following is some comparison data to give you a little perspective:

+   - An IPv4 address is a 32-bit length address that looks like this: 192.168.1.5
+   - An IPv6 address is a 128-bit length address that looks like this:
+     2001:AABB:CCDD:AB00:0123:4567:8901:ABCD

As you can see, the IPv4 address is much shorter, which obviously means there are fewer possibilities for unique IP addresses. What you don't see is how much longer IPv6 addresses really are. These examples portray IPv4 and IPv6 addresses as we are used to seeing them, in their finished forms. Really though, the IPv4 address is shown in decimal form, and the IPv6 in hexadecimal. IPv6 addresses are shown and used via hex so that the addresses are compressed as much as possible. In reality, if you dig around under the hood, an IPv6 address in its native 128-bit form might look something like this (and indeed, this is how it looks inside the actual packet):

```
00010000010000100001101100110000000000000000001000000000000000
00010000000000000000000000010000000000000000000000000000000001
```

That's an impressive set of digits, but not something that is very usable or friendly to the human eye. So rather than show the bits, what about an IPv6 address shown in decimal format, in the same way that IPv4 addresses have always been shown? In that case, an IPv6 address might look something like this:

```
192.16.1.2.34.0.0.1.0.0.0.0.0.0.1
```