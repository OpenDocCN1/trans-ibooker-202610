# 3.5 Running artificial intelligence where operational data is stored

Separate platforms for AI and business applications make deploying AI in production difficult,. The result is reduced end-to-end availability for applications and data access, risk of violating service level agreements because of overhead of sending operational data and receiving predictions from external AI platforms, and increased complexity and cost of managing different environments and external accelerators.

As the world is set to deploy AI everywhere, attention is turning from how fast to build AI to how fast to inference with the AI.

To support this shift, the Power E1080 delivers faster business insights by running AI in place with four Matrix Math Assist (MMA) units to accelerate AI in each Power10 technology-based processor-core. The robust execution capability of the processor cores with MMA AI acceleration, enhanced SIMD, and enhanced data bandwidth provide an alternative to external accelerators, such as GPUs, and related device management for execution of statistical machine learning and inferencing workloads. These features, combined with the possibility to consolidate multiple environments for AI model execution on a Power E1080 platform together with other different types of environments, reduces costs and leads to a greatly simplified solution stack for AI.

Operationalizing AI inferencing directly on a Power E1080 brings AI closer to data. This ability allows AI to inherit and benefit from the Enterprise Qualities of Service (QoS): reliability, availability, and security of the Power10 processor-based platform and support a performance boost. Enterprise business workflows can now readily and consistently use insights that are built with the support of AI.

The use of data gravity on Power10 processor-cores enables AI to run during a database operation or concurrently with an application, for example. This feature is key for time-sensitive use cases. It delivers fresh input data to AI faster and enhances the quality and speed of insight.

As no-code application development, pay-for-use model repositories, auto-machine learning, and AI-enabled application vendors continue to evolve and grow, the corresponding software products are brought over to the Power10 platform. Python and code from major frameworks and tools, such as TensorFlow, PyTorch, and XGBoost, run on the Power10 processor-based platform without any changes.

Open Neural Network Exchange (ONNX) models can be brought over from x86 processor-based servers or other platforms and small-sized virtual machines or Power Virtual Server (PowerVS), for deployment on Power E1080, which gives customers the ability to build on commodity hardware but deploy on enterprise servers.

Power10 processor-core architecture includes an embedded matrix math accelerator (MMA). This MMA is extrapolated for an E1080 system to provide up to 5x faster AI inference for FP32 to infuse AI into business applications and drive greater insights, or up to 10x faster AI inference by using reduced precision data types, such as Bfloat16 and INT8, when compared with a prior generation Power9-based server.

IBM optimized math libraries so that AI tools benefit from acceleration that is provided by the MMA units of the Power10 chip. The benefits of MMA action can be realized for statistical machine learning and inferencing, which provides a cost-effective alternative to external accelerators or GPUs.