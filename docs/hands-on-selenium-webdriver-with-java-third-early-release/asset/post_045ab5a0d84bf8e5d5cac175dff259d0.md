
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>${maven-surefire-plugin.version}</version>
    <configuration>
        <parallel>classesAndMethods</parallel>
        <threadCount>4</threadCount>
    </configuration>
</plugin>
</plugins>
</build>
```

The second way to implement parallelism with Maven Surefire is *forking*, i.e., creating multiple JVM processes. This option can be helpful if we need to prevent thread-level concurrency issues since different processes do not share memory space, as happens in multithreading. As a drawback, forking consumes more memory and has lower performance. To enable forking, we need to set the `forkCount` configuration property (again, in the `pom.xml` or as a system property) to a value higher than one, i.e., the number of JVM processes to be created. For example, the following command executes the tests of a Maven project using four JVM processes:

```bash
mvn test -DforkCount=4
```

## Gradle

Gradle also provides several ways to execute tests in parallel. First, it allows executing tasks in parallel in a multi-module project. There are two ways to enable this mode. First, by setting the property `org.gradle.parallel=true` in the configuration file `gradle.properties`. Second, using the option `--parallel` in the command, for instance, as follows:

```bash
gradle test --parallel
```

In addition, we can use the configuration property `maxParallelForks` in the Gradle configuration file to specify the maximum number of test processes to start in parallel. By default, Gradle executes a single test class at a time. We change this default behavior by setting a value higher than one for this parameter. In addition to a fixed value, we can specify the number of available CPU cores in the system as follows:

```groovy
maxParallelForks = Runtime.runtime.availableProcessors()
```

In the example repository, this property is enabled conditionally using a profile called `parallel` (see Appendix C). Therefore, we can use this profile using the command line as follows:

```bash
gradle test -Pparallel
```

## JUnit 4

JUnit provides a basic way to execute tests in parallel through the class `ParallelComputer`. This class accepts two Boolean parameters in its constructor to enable parallel test execution of classes and methods, respectively. Example 8-31 shows a test using this class.

### Example 8-31. Parallel test execution using JUnit 4

```java
public class ParallelJUnit4Suite {

    @Test
    public void runInParallel() {
        Class<?>[] classes = { ParallelJUnit4Test.class,
                ParallelJUnit4Test.class };
        JUnitCore.runClasses(new ParallelComputer(true, true),
                classes);
    }
}
```

- We specify which test classes are executed in parallel.
- We enable parallel test execution for test classes and methods.

## TestNG

A common way to specify parallel execution for tests in TestNG is through the configuration file `testng.xml`. The most relevant attributes to enable this mode in TestNG are:

- `parallel`: It specifies the mode for running tests in parallel. The alternatives are `methods`, `tests`, and `classes`.
- `thread-count`: It sets the default maximum number of threads for running tests in parallel.

Example 8-32 shows a basic configuration of `testng.xml` for test parallelism.

### Example 8-32. Parallel test configuration for TestNG

```xml
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">
<suite name="parallel-suite" parallel="classes" thread-count="2">
    <test name="parallel-tests">
        <classes>
            <class name="io.github.bonigarcia.webdriver.testng.ch08.parallel.Parallel1NGTest"/>
            <class name="io.github.bonigarcia.webdriver.testng.ch08.parallel.Parallel2NGTest"/>
        </classes>
    </test>
</suite>
```

We can use Maven or Gradle on the command line to run the previous parallel test suite as follows:

```bash
mvn test -Dsurefire.suiteXmlFiles=src/test/resources/testng.xml
gradle test -Psuite=src/test/resources/testng.xml
```

## JUnit 5

JUnit 5 allows different ways of executing tests in parallel. The following list summarizes the most relevant configuration parameters for this purpose:

- `junit.jupiter.execution.parallel.enabled`: Boolean flag to enable test parallelism (`false` by default).
- `junit.jupiter.execution.parallel.mode.classes.default`: To run test classes in parallel. The possible values are `same_thread` for single-threaded execution (by default) and `concurrent` for parallel execution.
- `junit.jupiter.execution.parallel.mode.default`: To run test methods in parallel. The possible values are the same as before (for test classes).

There are two ways of specifying these parameters. First, in the configuration file `junit-platform.properties` (which should be available in the project classpath). **Example 8-33** shows sample content of this file. Second, by using system properties and the command line. The following commands (Maven/Gradle) show how:

```bash
mvn test -Djunit.jupiter.execution.parallel.enabled=true
gradle test -Djunit.jupiter.execution.parallel.enabled=true
```

### Example 8-33. Parallel test execution using JUnit 5

```properties
junit.jupiter.execution.parallel.enabled = true
junit.jupiter.execution.parallel.mode.default = same_thread
junit.jupiter.execution.parallel.mode.classes.default = same_thread
```

In addition, the Jupiter programming model provides the annotation `@Execution` to change the parallelization mode for test classes or methods. To that aim, this annotation can be used at class level or method level and accepts two values: `ExecutionMode.CONCURRENT` (for parallel execution) and `ExecutionMode.SAME_THREAD` (for single-threaded execution). **Example 8-34** shows the structure of a test class contained in the example repository. Supposing that parallel test execution is enabled (as in **Example 8-33**), this class will be executed in parallel together with other tests that allow parallelization.

### Example 8-34. Parallel test execution using JUnit 5

```java
@Execution(ExecutionMode.CONCURRENT)
class ParallelJupiterTest {

    // Test logic
```

## Multi-Attention Fusion Network for Lightweight Single Image Super-Resolution

Zhen Huang, Panpan Wang, Zunkai Huang, Jiandong Gan, Yifan Zhang

School of Computer and Information, Hefei University of Technology, Hefei 230009, China  
663442489@qq.com, wangpanpan@hfut.edu.cn

**Abstract—** Deep learning-based single image super-resolution (SISR) algorithms have made significant achievements, yet their practical application is limited due to the huge number of parameters and computational overhead. In this paper, we propose a lightweight but efficient multi-attention fusion network (MAFN), which extracts features in a coarse-to-fine manner. Specifically, we propose a feature fusion group (FFG), consisting of a Feature Extraction Block (FEB), an Attention Fusion Block (AFB), and a Feature Refinement Block (FRB). The FEB uses two different branches with different receptive fields to extract coarse features. The AFB is designed to refine the features of different branches by fusing channel attention and spatial attention mechanisms. The FRB is used to further extract fine features from the fused features. We also introduce a Cross-layer Feature Attention Fusion (CFAF) module to fuse features from different FFGs, which can effectively improve the representation ability of the network. In addition, we propose a lightweight upsampling module to reduce the parameters introduced by the upsampling module. Extensive experiments show that our MAFN achieves superior performance in both reconstruction quality and visual effects compared to other state-of-the-art lightweight SISR methods.

*Index Terms—* super-resolution, lightweight network, attention mechanism, multi-attention fusion

## I. INTRODUCTION

Single image super-resolution (SISR) is a classical problem in the field of computer vision, which aims to reconstruct a high-resolution (HR) image from its corresponding low-resolution (LR) image. SISR technology has a wide range of applications in real-world scenarios, such as medical imaging, remote sensing, and video surveillance. In recent years, deep learning-based methods have achieved remarkable success in SISR. Dong et al. [1] first introduced convolutional neural networks (CNNs) into the SISR task and proposed the super-resolution convolutional neural network (SRCNN). Since then, a series of deep learning-based SISR methods have been proposed, such as very deep super-resolution (VDSR) [2], enhanced deep super-resolution (EDSR) [3], and deep residual channel attention network (RCAN) [4]. These methods have greatly improved the reconstruction performance of SISR.

However, the excellent performance of these methods is often accompanied by a large number of parameters and high computational complexity, which hinders their deployment on resource-constrained devices, such as mobile phones and embedded devices. To address this issue, researchers have proposed various lightweight SISR methods. For example, Ahn et al. [5] proposed the cascading residual network (CARN), which uses a cascading mechanism to reuse features and reduce parameters. Hui et al. [6] proposed the information multi-distillation network (IMDN), which uses an information multi-distillation mechanism to extract features efficiently. The efficient super-resolution network (ESRN) [7] and residual feature distillation network (RFDN) [8] further improve the performance of lightweight SISR. In addition, some methods [9]–[12] designed efficient attention mechanisms to improve the representation ability of lightweight networks by modeling the importance of features.

Despite the progress made by these methods, existing lightweight SISR methods still have some limitations. On the one hand, most of them use a single type of attention mechanism, which cannot fully capture the complex feature information in images. On the other hand, multi-scale information is often ignored in lightweight networks, limiting the representation ability of the network. To overcome these limitations, we propose a multi-attention fusion network (MAFN) for lightweight SISR. The main contributions of this paper are summarized as follows:
