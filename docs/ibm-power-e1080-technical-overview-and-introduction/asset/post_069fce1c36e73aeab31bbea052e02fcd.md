
- Platform Security ensures that the server is hardened against tampering, continuously protects its integrity, and ensures strong isolation among multi-tenant workloads. Without strong platform security, all other system security measures are at risk.
- Security Innovation for Modern Threats provides the ability to stay ahead of new types of cybersecurity threats by using emerging technologies.
- Integrated Security Management addresses the key challenge of ensuring correct configuration of the many security features across the stack, monitoring them, and reacting if unexpected changes are detected.

The Power E1080 is enhanced to simplify and integrate security management across the stack, which reduces the likelihood of administrator errors.

In the Power E1080, all data is protected by a greatly simplified end-to-end encryption that extends across the hybrid cloud without detectable performance impact and prepares for future cyber threats.

Power10 processor-core technology features built-in security integration:

- Stay ahead of current and future data threats with better cryptographic performance and support for quantum-safe cryptography and fully homomorphic encryption (FHE).
- Enhance the security of applications with more hardened defense against return-oriented programming (ROP) attacks.
- Simplified single-interface hybrid cloud security management without any required setup.
- Protect your applications and data with the most secure VM isolation in the industry with orders of magnitude lower Common Vulnerability Exposures (CVEs) than hypervisors related to x86 processor-based servers.

Also, workloads on the Power E1080 benefit from cryptographic algorithm acceleration, which allows algorithms, such as Advanced Encryption Standard (AES), SHA2, and SHA3 to run significantly faster than POWER9 processor-based servers on a per core basis. This performance acceleration allows features, such as AIX Logical Volume Encryption, to be enabled with low performance overhead.

## 3.4.1 Crypto engines and transparent memory encryption

Power10 processor technology is engineered to achieve significantly faster encryption performance with quadruple the number of AES encryption engines. In comparison to IBM POWER9 processor-based servers, Power E1080 is updated for today’s most demanding standards and anticipated future cryptographic standards, such as post-quantum and fully homomorphic encryption (FHE), and brings new enhancements to container security.

Transparent memory encryption is designed to simplify encryption and support end-to-end security without affecting performance by using hardware features for a seamless user experience. The protection that is introduced in all layers of an infrastructure is shown in Figure 3-5 on page 151.

![](img/7c8373945565ba432037f60bbf8458eb_164_0.png)

Figure 3-5 Protect data in memory with transparent memory encryption

## 3.4.2 Quantum-safe cryptography support

To be prepared for the Quantum era, the Power E1080 is built to efficiently support future cryptography, such as Quantum-safe cryptography and FHE. The software libraries for these solutions are optimized for the Power10 processor-chip instruction set architecture (ISA) and are or will be available in the respective open source communities.

Quantum-safe cryptography refers to the efforts to identify algorithms that are resistant to attacks by classical and quantum computers in preparation for the time when large-scale quantum computers are built.

Homomorphic encryption refers to encryption techniques that permit systems to perform computations on encrypted data without decrypting the data first. The software libraries for these solutions are optimized for the Power processor-chip instruction set architecture (ISA).

## 3.4.3 IBM PowerSC support

The Power E1080 benefits from the integrated security management capabilities that are offered by:

- IBM PowerSC
- The Power software portfolio for managing security and compliance on every Power processor-based platform that is running AIX
- IBM i or the supported distributions
- Versions of Linux

PowerSC is introducing more features to help customers manage security end-to-end across the stack to stay ahead of various threats. Specifically, PowerSC 2.0 adds support for Endpoint Detection and Response (EDR), host-based intrusion detection, block listing, and Linux.

## 3.5 Running artificial intelligence where operational data is stored

Separate platforms for AI and business applications make deploying AI in production difficult. The result is reduced end-to-end availability for applications and data access, risk of violating service level agreements because of overhead of sending operational data and receiving predictions from external AI platforms, and increased complexity and cost of managing different environments and external accelerators.

As the world is set to deploy AI everywhere, attention is turning from how fast to build AI to how fast to inference with the AI.

To support this shift, the Power E1080 delivers faster business insights by running AI in place with four Matrix Math Assist (MMA) units to accelerate AI in each Power10 technology-based processor-core. The robust execution capability of the processor cores with MMA AI acceleration, enhanced SIMD, and enhanced data bandwidth provide an alternative to external accelerators, such as GPUs, and related device management for execution of statistical machine learning and inferencing workloads. These features, combined with the possibility to consolidate multiple environments for AI model execution on a Power E1080 platform together with other different types of environments, reduces costs and leads to a greatly simplified solution stack for AI.

Operationalizing AI inferencing directly on a Power E1080 brings AI closer to data. This ability allows AI to inherit and benefit from the Enterprise Qualities of Service (QoS): reliability, availability, and security of the Power10 processor-based platform and support a performance boost. Enterprise business workflows can now readily and consistently use insights that are built with the support of AI.

The use of data gravity on Power10 processor-cores enables AI to run during a database operation or concurrently with an application, for example. This feature is key for time-sensitive use cases. It delivers fresh input data to AI faster and enhances the quality and speed of insight.

As no-code application development, pay-for-use model repositories, auto-machine learning, and AI-enabled application vendors continue to evolve and grow, the corresponding software products are brought over to the Power10 platform. Python and code from major frameworks and tools, such as TensorFlow, PyTorch, and XGBoost, run on the Power10 processor-based platform without any changes.

Open Neural Network Exchange (ONNX) models can be brought over from x86 processor-based servers or other platforms and small-sized virtual machines or Power Virtual Server (PowerVS), for deployment on Power E1080, which gives customers the ability to build on commodity hardware but deploy on enterprise servers.

Power10 processor-core architecture includes an embedded matrix math accelerator (MMA). This MMA is extrapolated for an E1080 system to provide up to 5x faster AI inference for FP32 to infuse AI into business applications and drive greater insights, or up to 10x faster AI inference by using reduced precision data types, such as Bfloat16 and INT8, when compared with a prior generation Power9-based server.

IBM optimized math libraries so that AI tools benefit from acceleration that is provided by the MMA units of the Power10 chip. The benefits of MMA action can be realized for statistical machine learning and inferencing, which provides a cost-effective alternative to external accelerators or GPUs.

### 3.5.1 Train anywhere, deploy on Power E1080

IBM Power10 processor-based technology addresses challenges through hardware and software innovation. Models that are trained on any cloud that is based on Power or x86 processor-based servers (even with different endian-ness) can be deployed on the Power E1080 and run without any changes.

Power10 cores are equipped with four Math Engines for matrix and tensor math. Applications can run models with colocated data without the need for external accelerators, GPUs, or extra AI platforms. Power10 technology uses the “train anywhere, deploy here” principle to operationalize AI.

A model can be trained on a public or private cloud (see Figure 3-6) by using the following procedure:

1. The model is registered with its version in the so-called *model vault*, which is a VM or LPAR with tools, such as Watson Openscale, BentoML, or Tensorflow Serving.
2. The model is pushed out to the destination (in this case, it is a VM or an LPAR running a database with an application) and the model might be used by the database or the application.
3. Transactions that are received by the database and application trigger model execution and generate predictions or classifications. These predictions also can be stored locally. For example, these predictions can be the risk or fraud that is associated with the transaction or product classifications to be used by downstream applications.
4. A copy of the model output (prediction or classification) is sent to the model operations (ModelOps) engine for calculation of drift by comparison with Ground Truth.
5. If the drift exceeds a threshold, the model retrain triggers are generated.
6. Retrained models are then taken through steps 1 - 5.

![](img/7c8373945565ba432037f60bbf8458eb_166_0.png)

Figure 3-6 Operationalizing AI

By using a combination of software and hardware innovation, Power E1080 can meet the model performance, response time, and throughput KPIs of databases and applications that are infused with AI.
