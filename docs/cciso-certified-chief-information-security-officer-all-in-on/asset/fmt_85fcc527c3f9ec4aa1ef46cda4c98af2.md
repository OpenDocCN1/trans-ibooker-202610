
# Data Life Cycle

As part of the data life cycle, data may need to be archived in accordance with the organization's data retention policy and requirements. *Archival* is when data is taken out of production use and retained for future use or reference. This is often to assist with managing storage space and to ensure long-term retention requirements are met. Retention requirements are influenced by the organization's internal requirements (such as the potential need to access or restore the data in the future) as well as external requirements (such as laws, regulations, or other compliance drivers). Archival and retention may apply to both digital/electronic information and physical documents.

## Destruction

When data has passed its useful lifetime and retention requirements, it must be securely destroyed. As security professionals well know, when data is erased by pressing the DELETE key on a computer, that data storage location on the drive is essentially being marked as free for use by the operating system. This tells the OS that that sector of the drive, cluster, or block (depending on what filesystem is being used) is now free to be re-consumed. However, all the "deleted" data is still on the drive. This method is known as *erasure*. Since all the data is still on the media, erasure is not a secure method of data disposal. When media is securely cleared of its contents, it is said to be *sanitized*. This means erasing information so that it is not readily retrievable using routine OS commands or commercially available forensic/data recovery software. Media can be sanitized in several ways: purging, overwriting/zeroization, degaussing, and physical destruction. These methods are used to securely sanitize data in assets such as hard drives and other storage media. Each of these methods is defined and discussed here:

- **Purging** means making information unrecoverable even with extraordinary effort such as physical forensics in a laboratory. Purging is required when media either will be removed from the physical confines where the information on the media was allowed to be accessed or will be repurposed to a different compartment.

- **Overwriting/zeroization** deals with some sort of data overwrite. Several common methods are used:

  - **Single pass** Data is overwritten with *one* pass of 1's or 0's.
  - **US Department of Defense (DoD) method** Data area is overwritten with *three* passes; once with all 0's, then with all 1's, and finally once with pseudo-random data.
  - **US National Security Agency (NSA) erasure algorithm** Data is overwritten *seven* times, first with all 0's, then with all 1's, and so on.
  - **Gutman method** Data area is overwritten 35 times.

- **Degaussing** is a process that demagnetizes magnetic media so that a very low residue of magnetic induction is left on the media. This entails exposing the media to an extremely powerful magnetic field. Degaussing sanitizes legacy magnetic media sufficiently for it to be reused; however, degaussing modern media such as disks and tape with chipsets will render them inoperable (essentially destruction).

- **Physical destruction** is the process of destroying the media so that it cannot be reused and the data cannot be accessed. This is the most secure method of data destruction. This includes shredding, crushing, burning, disintegration, and dissolving in chemical compounds. Physical destruction is also the method that is used for physical documents, as you cannot purge, zeroize, or degauss paper documents.

### Weak Link: The Dumpster!

After their useful life is over, old computers along with their hard drives and other magnetic storage media are replaced and thrown away. To mitigate the risk that residual data is on these assets, several methods for data sanitization are used, including purging, zeroization, degaussing, and physical destruction of media. However, digital data is not the only type of information that must be protected. Other forms of information, such as paper, also require secure disposal to protect against dumpster diving. *Dumpster diving* is the act of searching through trash (dumpsters, office trashcans, recycling bins, and so on) to locate valuable information that was discarded without proper sanitization (such as sensitive paper documents or non-sanitized physical media). The best method for the disposal of sensitive documents is physical destruction such as shredding or burning. This will protect against the dumpster diver.

## Identity and Access Management

*Identity and access management (IAM)* refers to systems and processes put into place to manage and control access to enterprise resources. IAM involves the management and implementation of *who* is accessing *what* and what they can *do* with it. This suggests three key terms:

- **Subject** The entity that actively accesses an object
- **Object** The entity that is accessed by a subject
- **Access** The actions that subjects can perform or impose on an object

A *subject* is an active entity such as an individual, system, process, or device that accesses an object. The management and verification of the subject's identity is where identity management comes into play. *Identity management* is focused on identifying who the subject is and verifying (authenticating) they are who they say they are. An *object* is an entity such as a network, computer, file, device, record, table, process, domain, database, or program containing information that is accessed by a subject. An object is typically a resource that stores, processes, or sends/receives data. *Access* is the action that a subject imposes on an object such as read, modify, delete, create, or execute. This is where access management comes into play. *Access management* systems manage which objects subjects can access and what they can do with them.

### Hardcopy Example

The concept of access can be illustrated by considering how it might apply not just to digital data but also to hardcopy data. For example, if an organization stores sensitive documents in a safe, the organization must limit who has access to the safe through the use of a combination lock only provided to authorized employees (access management). In order for someone to be provided with the combination, the organization must know who the person is and verify their identity to ensure they are who they say they are (identity management). In this example the authorized employee with access to open the safe using the combination is the subject and the safe is the object.

This section discusses the following topics:

- Identity and access management fundamentals
- Identity management technologies
- Authentication factors and mechanisms
- Access control principles
- Access control models
- Access control administration
- Identify and access management life cycle

### Identity and Access Management Fundamentals

Identity management is focused on identifying and verifying the subject's identity, whereas access management is focused on managing the subject's access to objects, defining the actions that can be performed, and tracking of these actions. These are enabled through identification, authentication, authorization, and accountability/auditing as defined next:

- **Identification** The identification function identifies who the subject is. In many systems the subjects supply information to identify themselves, such as a username, account number, or user ID.

- **Authentication** The authentication function verifies the subject is who they say they are. Verification of an identity is typically performed though the use of a password, passphrase, token device, smart card, cryptographic key, or biometric. These are methods of authentication.

- **Authorization** The authorization function grants access to an object based on predefined access management rules that establish which operations subjects are allowed to carry out on objects.
