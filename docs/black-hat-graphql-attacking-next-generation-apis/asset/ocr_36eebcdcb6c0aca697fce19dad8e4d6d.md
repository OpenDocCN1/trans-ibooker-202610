can also lead to server-side request forgery (SSRF) vulnerabilities (more on SSRF vulnerabilities in Chapter 9). In addition, since the application inserts the client input directly into the wget command, we could introduce any shell command we desire. We could also use the semicolon (;) character to break or separate the wget command and start a new command, effectively performing an OS command injection. This could lead to complete server compromise.

# Manual Testing in DVGA

In GraphQL, OS command injection can happen if a resolver function accepts arguments from a GraphQL field without implementing the necessary verifications on the input. Let’s explore what this looks like in DVGA.

Returning to the schema documentation we reviewed earlier, we have four fields of interest, which all start with the word system: systemUpdate, systemHealth, systemDiagnostics, and systemDebug. While field names can differ from one application to another, the word system often hints at the use of system shell commands under the hood, so exploring those for OS command injections is worthwhile.

If you’ve ever performed a penetration test on your home router, you’ll know that its debug or diagnostics page is probably the most interesting place to look for impactful vulnerabilities. OS command injections often exist in these interfaces, as they use network utilities such as ping or traceroute under the hood. Home routers aren’t particularly famous for their security; they hardly ever check input for dangerous characters and are often vulnerable to OS command injection.

In this section, we’ll focus on systemDebug. Run the following in Altair to see the kind of response we get:

```
query {
  systemDebug
}
```

If you’ve done a bit of Linux system administration, you may recognize the following output excerpt; it comes from the ps command, which displays information about running system and user processes:

```
"systemDebug": "    PID TTY          TIME CMD\n 11999 pts/1    00:00:00 bash\n 14050 pts/1    00:00:00 python3\n 14055 pts/1    00:00:03 python3\n 14135 pts/1    00:00:00 sh\n 14136 pts/1    00:00:00 ps\n"
```

Open the Docs page in Altair. Under Queries, you’ll notice that systemDebug takes a single argument, named arg, of type String, which seems promising. Does the GraphQL query resolver send this argument directly to the ps command? Let’s find out:

```
query {
  systemDebug(arg:"ef")
}
```