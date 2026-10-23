# Table 7-1. (continued)

| No. # | Item to be checked | Description | Yes/No | Notes |
|---|---|---|---|---|
| 3 | Check for direct breaks of system security. | Checking for untrusted input will lead to a direct breach of the application security. With this step, you protect the application against malicious users and attackers exploiting your program using metachars.

✓ system()
✓ popen()
✓ fork(2)
✓ exec(2)
✓ s_popen()
✓ HXproc_* [2] |  |  |
| 4 | Check for the wrong size of parameters and getting unexpected results. | When complex programs are written, such as the implementation of SHA-256 from Chapter 2, Listing 2-1, assigning a wrong size to one of the parameters or doing a wrong arithmetic operation can cause a serious pitfall and a fix should immediately be provided. Make sure that the size allocated for the parameters is the same size on the destination side. As a best practice, especially in implementing cryptography algorithms, it is better to work with size_t type. Be type-safe and don't create overflows.

✓ strncpy()
✓ strncat()
✓ snprintf() |  |  |