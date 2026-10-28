elements in our design recipes.

# # Establishing a Foundation with Hypermedia Designs

There are three general ideas behind the design recipes in this chapter:

+   - An agreed communication format to handle connections between networked machines
+   - A model for interpreting data as information
+   - A technique for telling machines, at run-time, just what actions are valid.

All the recipes in this book are devoted to the idea of making useful connections between application services running on networked machines. The most common way to do that today is through TCP/IP at the packet level and HTTP at the message level. There’s an interesting bit of history behind the way the United States defense department initially designed and funded the first machine-to-machine networks (ARPANET) which eventually became the Internet we use today. And it involves out space aliens. In the 1960s, as the U.S. was developing computer communications,it was the possibility of encountering aliens from outer space that drove some of the design choices for communicating between machines.

Along with agreed communication formats for inter-machine communications, the work of organizing and sharing data between machines is another theme in this chapter. To do this, we’ll dig a bit into information architecture (IA) and learn the value of ontologies, taxonomies, and choreography. The history of IA starts about the same time Roy Fielding was developing his REST software architecture style and was heavily influenced by the rise of Tim Berners-Lee’s World-Wide Web of HTTP and HTML. In this chapter, we’ll use IA as an organizing factor to guide how we describe service capabilities using a shared vocabulary pattern.