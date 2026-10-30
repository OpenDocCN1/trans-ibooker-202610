This chapter will show you how to jumpstart your observability journey with the flexibility and power of OTel. You can use these lessons to quickly instrument your applications and use that data in any number of observability solutions.

# # Instrumentation Using Code-Based Examples

At the time of publication, OTel supports instrumenting code written in many languages (including Go, Python, Java, Ruby, Erlang, PHP, JavaScript, .NET, Rust, C++, and Swift), and also includes a common message standard and collector agent.

To provide practical examples of instrumentation rather than confining ourselves to abstract concepts, we need to utilize one specific language that OTel supports. We use the Go programming language for these examples because it is the language Kubernetes is written in and a lingua franca of modern distributed microservices and cloud native systems. However, you can apply these concepts to any language because OTel offers standard terminology and API design (when possible) in each language it supports.

You shouldn’t need to be a Go programmer to understand these examples. However, we’ll also explain what each of the following code snippets does in detail.