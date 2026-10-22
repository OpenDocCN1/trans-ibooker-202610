Many of the tools I will talk about are used in conjunction with one another as there is not silver bullet tool that will solve all of your security issues. For instance, static analysis tools are great to get an understanding of “hot spots” where the application appears to be weak from a security point of view. It can also be input into threat modeling exercises to allow the participants to focus on mitigations in trouble areas.

Each SAST vendor has a different way of scanning however most follow a pattern of compile, model extraction, pattern matching, and flow analysis as shown in figure 3.5

# Generic Static Application Security Testing Flow

![](bbox=[0.2, 0.33, 0.8, 0.48])

Figure 3.5 High Level SAST Process

There are certainly strengths and weaknesses with SAST tools. Some of the primary considerations are that SAST tools are usually bound by limitations in the languages that they cover which means that if you work in most large organizations, you will have several languages that are used by the different teams. You may be forced to use more than one SAST product to get coverage of all the languages in the organization. Additionally, SAST products are the most notorious products for producing a lot of false positives. You may be able to tune the product to reduce this noise, but this will take time and coordination with the security and engineering teams. Lastly, the SAST tools are only able to see the static code and cannot see how it runs in an environment. This limits its ability to locate potential run-time security issues like parameter tampering.

There are a few things that SAST does well. It is ideal for locating low hanging fruit like hard coded secrets or passwords as well as locating poor secure code practices like SQL injection type flaws or poor encryption methods. It can also pinpoint exactly in the code where an issue will manifest and give recommendations on how to fix it. This is extremely helpful for developers, and security folks, who want to fix a security issue as close to the code as possible.

Ultimately, SAST tools are as close to the developer as it gets. For those, myself included, that want to ensure that the right tools are available to the developer while they are creating the code, nothing really beats SAST. This needs to be weighed with the strengths and weaknesses mentioned above. Static analysis tools are decent at finding vulnerabilities early,