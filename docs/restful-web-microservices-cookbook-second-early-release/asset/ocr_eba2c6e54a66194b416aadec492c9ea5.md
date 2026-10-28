+   - RDFS : Resource Description Format Schema ⁴
+   - OWL : web Ontology Language ⁵
+   - DCAP : Dublin Core Application Profiles ⁶
+   - ALPS : Application-Level Profile Semantics ⁷

Since I am a co-author on the ALPS specification, you’ll see lots of examples of using ALPS as the vocabulary standards document. You may also be using a format not listed here — and that’s just fine. The important thing is that you and the services your client application interacts with agree on a vocabulary format as a way to describe the problem domain.

# Example

Expressing a service’s domain as a set of vocabulary terms allows both client and service to *share understanding* in a generic, stable manner. This was covered in Recipe 2.4. Here’s a set of vocabulary terms for a simple domain:

```
# Simple ToDo

## Purpose
We need to track 'ToDo' records in order to improve both
timeliness
and accuracy of customer follow-up activity.

## Data
In this first pass at the application, we need to keep track of
the
following data properties:

* **id** : a globally unique value for each ToDo record
* **body** : the text content of the ToDo record

## Actions
This edition of the application needs to support the following
operations:

* **Home** : starting location of the service
* **List** : return a list of all active ToDo records in the system
* **Add** : add a new ToDo record to the system
* **Remove** : remove a completed ToDo record from the system
```