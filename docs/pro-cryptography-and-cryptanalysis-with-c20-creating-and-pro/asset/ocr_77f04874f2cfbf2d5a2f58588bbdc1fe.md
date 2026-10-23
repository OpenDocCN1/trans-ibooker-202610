![](bbox=[0.32, 0.1, 0.715, 0.35])

# Figure 12-1. The structure of the SEAL.sln solution

The folder used for C++ development is the native folder from the solution. To use SEAL library in your own C++ application, you need first to generate the seal.lib library. To do this, you need to build the SEAL project from Figure 12-1. From the Toolbar, pick Release configuration and x64 platform (Figure 12-2(a) and 12-2(b)), then right-click the SEAL project and choose Build. The Release configuration is needed because things go faster than in the Debug configuration and you actually just need to generate the seal.lib, not debug it. Right-click the SEAL project under the native folder and choose Build or Rebuild.

![](bbox=[0.105, 0.59, 0.93, 0.78])

# Figure 12-2(a). Choosing the configuration for SEAL building