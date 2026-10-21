# EyeWitness

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

# GraphQL Cop

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