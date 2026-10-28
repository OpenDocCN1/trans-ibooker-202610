There are currently no pre-defined link relation values for specifying a particular SSD format (OpenAPI, AsyncAPI, etc.). If you use some terms for this (as in the example above), but sure to include these terms in your API’s shared vocabulary (see Recipe 4.7, Recipe 4.8).

There is not agreed standard SSD format for hypermedia-style APIs. For now, I recommend including an ALPS document as part of the API’s profile instead (see Recipe 4.7).

# Related

+   - Chapter 2
+   - Chapter 3
+   - Chapter 4
+   - [Link to Come]

# 4.10 Publishing API Metadata

As the number of application interfaces on the open web grows, there is a need for common format and common vocabulary for describing metadata about the API. This is especially important when service providers want to communicate important details about the API without the need for physical meetings, interactions, and/or bespoke description and documentation efforts. Luckily, there is a strong contender for carrying this information: the APIs.json specification. 65

# Problem

How can we represent important metadata about a service interface (supported formats, vocabularies, documentation, security, etc.) in a standardized way that allows API consumers to easily find and understand the meta-level details of the interface?

# Solution