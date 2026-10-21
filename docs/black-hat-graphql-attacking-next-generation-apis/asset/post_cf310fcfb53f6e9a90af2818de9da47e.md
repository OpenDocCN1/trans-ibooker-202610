
exploiting these vulnerabilities and can spawn a custom interactive shell, which penetration testers can use to gain a foothold in remote servers.

Commix should be preinstalled in Kali by default, but to ensure that it is indeed available and working properly, run the following set of commands:

```
# sudo apt install commix -y
# commix -h

Usage: commix [option(s)]

Options:
  -h, --help              Show help and exit.

General:
  These options relate to general matters.

  -v VERBOSE              Verbosity level (0-4, Default: 0).
  --version               Show version number and exit.
```

## graphql-path-enum

Written in Rust and developed by dee_see (@dee_see), graphql-path-enum is a security testing tool that finds various ways to construct queries that reach a specific piece of data. By doing so, it arms hackers with information that could assist them in identifying authorization flaws. We’ll discuss GraphQL authorization vulnerabilities in Chapter 7.

Install graphql-path-enum by running the following commands:

```
# cd ~
# wget "https://gitlab.com/dee-see/graphql-path-enum/-/jobs/artifacts/v1.1/raw/target/release/graphql-path-enum?job=build-linux"
-O graphql-path-enum
# chmod u+x graphql-path-enum
```

Verify that it can successfully run with its new permissions by passing it the -h flag:

```
# ./graphql-path-enum -h

graphql-path-enum

USAGE:
  graphql-path-enum [FLAGS] --introspect-query-path <FILE_PATH> --type <TYPE_NAME>

FLAGS:
  --expand-connections    Expand connection nodes
    (with pageInfo, edges, etc. edges), they are skipped by default.
  -h, --help              Prints help information
  --include-mutations     Include paths from the Mutation node.
    Off by default because this often adds a lot of noise.
  -V, --version           Prints version information
```

## EyeWitness

EyeWitness is a web-scanning tool developed by Chris Truncer and Rohan Vazarkar that is capable of capturing screenshots of target web applications. When scanning many websites in a penetration test, you'll often find it useful to visually identify what's running on them. EyeWitness achieves this using a command line-based web browser (also called a headless browser) under the hood, which allows it to load dynamic web content, such as content loaded dynamically using JavaScript.

Install EyeWitness with the following command:

```
# sudo apt install eyewitness -y
# eyewitness -h
```

Protocols:
  --web          HTTP Screenshot using Selenium

Input Options:
  -f Filename      Line-separated file containing URLs to capture
  -x Filename.xml  Nmap XML or .Nessus file
  --single Single URL  Single URL/Host to capture
  --no-dns

## GraphQL Cop

We developed GraphQL Cop, a dedicated GraphQL security auditing utility based on Python. GraphQL Cop audits GraphQL servers for information disclosure and DoS-based vulnerabilities. In later chapters, we will use this tool to check whether GraphQL servers are protected against common attacks.

Install GraphQL Cop with the following set of commands:

```
# sudo apt install python3-pip -y
# git clone https://github.com/dolevf/graphql-cop.git
# cd graphql-cop
# pip3 install -r requirements.txt
# python3 graphql-cop.py -h
```

Options:
  -h, --help      show this help message and exit
  -t URL, --target=URL  target url with the path
  -H HEADER, --header=HEADER  Append Header to the request '{"Authorization": "Bearer eyjt}"' 
  -o FORMAT, --output=FORMAT  json
  -x, --proxy     Sends the request through http://127.0.0.1:8080 proxy
  -v, --version   Print out the current version and exit.

## CrackQL

We developed CrackQL, a specialized brute-forcing tool for GraphQL that uses GraphQL language features to better optimize brute-force attacks against API actions that may require authentication. We will use this tool in Chapter 7, when we perform dictionary-based attacks against our GraphQL target. Install CrackQL as follows:

```
# git clone https://github.com/nicholasaleks/CrackQL.git
# cd CrackQL
# pip3 install -r requirements.txt
# python3 CrackQL.py -h
```

```
Options:
  -h, --help              show this help message and exit
  -t URL, --target=URL    Target url with a path to the GraphQL endpoint
  -q QUERY, --query=QUERY Input query or mutation operation with variable payload markers
  -i INPUT_CSV, --input-csv=INPUT_CSV Path to a csv list of arguments (i.e. usernames, emails, ids, passwords, otp_tokens, etc.)
```

Once you’ve installed all of these tools, we highly encourage you to take a snapshot of your Kali virtual machine to ensure that its state is saved. You’ll then be able to restore it should it break in the future.

## Summary

Let’s summarize what you currently have in your lab: graphical and command line HTTP clients that can interact with GraphQL, a working Docker environment for deploying containers, and the DVGA target application.

This chapter briefly discussed how these tools work under the hood and the needs they fill, such as information gathering, server fingerprinting, network and application scanning, vulnerability assessments, and GraphQL auditing. You’ll explore their use in more depth in the remaining chapters.

This lab is an essential part of this book, but it might also prove valuable for your next real-world penetration test. We encourage you to keep an eye on the Black Hat GraphQL GitHub repository (https://github.com/dolevjf/Black-Hat-GraphQL.git), where we maintain a list of current and future GraphQL security tools to help you keep your lab up to date.

## 3

## THE GRAPHQL ATTACK SURFACE

![](img/2f244b00cd9f8f37cfc22f928aaa471a_66_0.png)

In this chapter, we first explore GraphQL’s language and type system through the eyes of a hacker. Then we provide an overview of the common weaknesses in GraphQL.
We hope you have your imaginary black hat handy, because you’re about to learn how a feature can turn into a weakness, how a misconfiguration can turn into an information leak, and how implementation design flaws can lead to DoS opportunities.

## What Is an Attack Surface?

An *attack surface* is the sum of all possible attack vectors an adversary can use to compromise the confidentiality, integrity, and availability of a system. For example, imagine a physical building with a front door, a side door, and

multiple windows. As attackers, we view each of these windows and doors as a possible opportunity to gain unauthorized access to the building.

Typically, a system has a higher risk of an attack succeeding when its attack surface is large, such as when it consists of many applications, databases, servers, endpoints, and so on. The more windows and doors a building has, the higher the probability that one of those entry points is unlocked or insecure.

Attack surfaces change over time, especially as systems and their environments evolve. This is particularly true in cloud environments, where infrastructure is elastic. For example, a server could live for only a limited amount of time, or an IP address could change, sometimes multiple times a day.

Let’s review all the windows and doors in GraphQL and highlight possible attack vectors we can use to unlock them. Understanding these concepts will aid you in the next chapters, where we dive deeper into offensive security.

## The Language

For the purposes of discussing GraphQL’s attack surface, we will break its specification into two sections; its language and its type system. We begin by covering the language, used to make requests to a GraphQL API server, from a client’s point of view. Next, we’ll review its type system from a server’s point of view. You can learn about these concepts and other GraphQL internals by using the GraphQL specification; here, we intend to distill only the parts that will equip you with enough knowledge to test GraphQL attack vectors in future chapters.

The GraphQL language comprises many useful components that clients can leverage. At first glance, the way these elements are represented within requests may appear confusing. Figure 3-1 is a sample GraphQL query whose components are explained in Table 3-1.

![](img/2f244b00cd9f8f37cfc22f928aaa471a_67_0.png)

Figure 3-1: A sample GraphQL query
