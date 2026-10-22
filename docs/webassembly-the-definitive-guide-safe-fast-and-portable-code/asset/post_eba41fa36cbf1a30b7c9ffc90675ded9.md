
The typical programmer thinks of her job as writing software that runs on computers containing a Central Processing Unit (CPU), main memory, a storage system, a display and some kind of input devices. While this still covers a significant amount of software development, we have a richer ecosystem of computational runtimes than most people realize. Computers, tablets, game consoles, networking equipment, smart phones, watches, embedded systems, Internet of Things (IoT) devices, and systems on chip (SOC) provide a menagerie of hardware systems that execute software.

Herb Sutter of C++ fame has written an excellent essay² called “Welcome to the Jungle” that is worth your time. In it, he highlights the impact of hardware developments on the software industry over the last several decades. For approximately 30 years, Moore’s law³ translated higher density into faster chips. If your software was slow, you simply waited 18-24 months and it would get faster.

Once we stopped being able to make faster, more complex chips, we used the extra density to make more simple chips. Multicore systems became the norm. Unlike the previous “free lunch” period, developers now had to write crazy concurrent code to benefit from the extra processing power and not every problem fits this mold nicely. Also, this is tricky code and easy to get wrong. This has largely pushed us toward functional programming languages and immutable data structures.

Other developments include the emergence of cloud computing to meet elastic demand, edge computing to distribute it geographically for low-latency customer experiences, and heterogenous computing environments. This includes Graphical Processing Units (GPUs), Field Programmable Gate Arrays (FPGAs) and Application-Specific Integrated Circuits (ASICs).

Tied up in all of this is the idea that it takes time and power to compute things. A big part of a successful IT strategy moving forward is going to be about minimizing time costs, power costs and latency costs. This is going to influence where things run. It makes sense to push large amounts of data into the cloud for elastic, bursty training sessions. But if this results in large models, those are not as easily distributed to desktop/mobile experience because of size. At the same time, we do not want internet sensors and car brakes making cloud calls at crucial times.

We are facing an absolute explosion of data which is going to make all of this more crucial. In order to process data in reasonable amounts of time, we need access to hardware acceleration. This parallelizes the math so it is tractable. It is in this world that we finally can explore the concept of JavaScript-based machine learning in the browser.

## Playground

Tensorflow Playground⁴ is an experimental environment for non-experts to gain intuition about how neural networks work through direct manipulation⁵. The idea is that these models are increasingly driving systems with real-world consequences, but the background needed to understand them is beyond the typical non-academic researcher. By working with visual representations, non-technical users are able to gain a sense of cause and effect.

The problem is that the JavaScript environment is single-threaded and the numerics support is not great for the kind of math that training neural networks requires. The Playground environment allows you to change all hyperparameters of the training process via simple user interface actions. While it is easy and fun to experiment with changing these parameters, they can have significant impacts on the quality of the results. In order to see the effect quickly and in real time, the runtime needs to recalculate significant portions of the code otherwise the whole point of direct manipulation is lost. Fortunately they were able to eke out enough performance to make it all work by relying on the presence of WebGL in the browser. We will discuss this idea more below.

## Tensorflow.js

Out of the success of the Playground came an appreciation of what was necessary and possible to bring deep learning systems to the browser. It may strike you as a bit odd to want to run such computationally-intensive systems on potentially lower-end devices such as phones or tablets. Even just running in the browser on a powerful desktop machine is a slightly odd concept. There are quite a few benefits to the idea though.

Unsurprisingly, the experience of clicking on a link and downloading a deep learning-based application is a lot easier than having to install a potentially large number of required libraries. There is zero installation to load a web page. It is easier to share research and practical applications when it is this easy. This broadens the potential for interaction among deep learning researchers and makes it easier to target endusers with applications that employ these capabilities.

Given the popularity of JavaScript, it is a bit of a steep request that web developers learn Python in order to do machine learning. There is an overwhelming amount of open source JavaScript code for building all manner of software systems. Being able to take advantage of it all would be a benefit to developers of machine learning systems in the other direction.

Our phones and tablets have quickly become something much more than mobile portable electronic devices. They have become diagnostic tools, part of our banking systems, methods of identification and more. New software is being written to detect the onset of dementia or the presence of strokes by analyzing video from their cameras. Both for the quality of the user experience and privacy law compliance with respect to the results of diagnostic outcomes, being able to push apps to the device will be much less encumbered by regulatory burdens than trying to take data off of the device.

Finally, many of the devices that these downloaded apps might run on have powerful and sophisticated GPUs available for use. Not only could it be possible to do deep learning systems in the browser, it might actually perform well.

So, why are we discussing all of this?

The design of the Tensorflow.js framework is elegant and results in clean APIs that work across a wide range of devices. Rather than limiting the implementation to what is available everywhere, they chose to create a pluggable backend to cover the widest number of systems.

The basic version of the backend is a CPU-based JavaScript implementation that will run anywhere. All of the code executes directly on the CPU without benefit of optimizing instruction sets such as the Advanced Vector Extensions (AVX)⁶. It does not perform effectively anywhere but it will run basically everywhere. Other options are better first choices, but this is a decent fallback position.

The next major backend is accelerated by WebGL. There is no direct support for accessing GPUs from JavaScript, but there is via WebGL which is supported in nearly every browser. By pretending to do 3D graphics, they can execute calculations on the GPU and write the results into the textures. This produces a fast and convenient implementation that runs well on just about every major modern browser. It is similar to what was done with the Tensorflow Playground application mentioned earlier.

### NOTE

This is not as strange a scenario as it sounds. Prior to the availability of GPU computation libraries such as CUDA, OpenCL, Metal and Vulkan, researchers would do this with OpenGL to get access to comparatively cheap computation. These APIs that specifically allow the use of GPUs for arbitrary computation was part of what accelerated the machine learning and deep learning systems we enjoy these days.

Servers with deep learning needs can be implemented in Node.js, so the next backend is designed to run in a freer environment than the browser. Apps running in Node.js can read from and write to the filesystem, load and use native libraries, and communicate directly with the normal native Tensorflow libraries. These can take advantage of multicore systems, GPUs or other hardware accelerating devices such as Google’s Tensor Processing Units (TPUs).
