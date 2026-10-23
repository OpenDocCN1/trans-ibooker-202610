the search query and the index, and the output is R_(ω, λ(u)), which includes a set of identifiers of the data items d_j ∈ D_ω, λ(u) that contain the query keyword ω such that λ(d_j) ≤ λ(u), where λ(u_j) is the access level of the user that triggered the search query, or a failure symbol φ.

+   6. (K_O) ← RevokeUser(u, K_O, PP). The data owner O runs this probabilistic algorithm to revoke a specific user from the system. The input values are the user’s id, the data owner’s secret keys and the server, while the output is new keys for the owner and server.

The searchable encryption scheme designed for this chapter is correct if for all k ∈ N, for all K_O, K_S outputted by KeyGen(1^λ, P), for all D^aug, for all I_D that is outputted by BuildIndex(D^aug, K_O), for all ω ∈ Δ, for all u ∈ U for all K_u outputted by AddUser(K_O, u, λ(u), PP), Search(I_D, T_ω, λ(u)) = D_ω, λ(u).

Pseudocode 11-1 presents a sketch for the practical implementation of the searchable encryption scheme proposed in Figure 11-1. Note that the implementation is purely demonstrative as the implementations (frameworks, libraries, etc.) for searchable encryption do not exist at this moment.

# Pseudocode 11-1. Guidelines for Implementing a Searchable Encryption Scheme

```
#include <iostream>
#include <fstream>

class KeyGeneration
{
// Step 1
// The data owner runs the algorithm
// from KeyGeneration step (algorithm)

// global variables
public: string securityParameter;
       string ownerID;
       string policyContent;
       string serverIdentity;
```