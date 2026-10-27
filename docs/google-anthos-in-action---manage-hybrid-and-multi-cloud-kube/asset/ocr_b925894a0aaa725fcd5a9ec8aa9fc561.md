# 5 Anthos Service Mesh: security and observability at scale

# by Onofrio Petragallo

# This chapter covers

+   - Sidecar proxy and proxyless architectures
+   - An introduction and the main features of Istio
+   - Security and observability with Istio
+   - What is Anthos Service Mesh
+   - Practical example with code

One of the key components to being cloud native is to break up your application into microservices. This means that an application that may have run on a single server, now has multiple services, backed by multiple pods, that are separate components. As applications scale out their services, it becomes difficult to troubleshoot issues that you may encounter with the application. Wlth this added complexity, we needed a tool that would help organize, secure, and add resilience to the expanding complexities that microservices introduced. Another important problem that a service mesh could fix is the fact that enterprises often have a huge number of microservices and aren’t always able to control, manage and observe them.

In this chapter, we will discuss Anthos Service Mesh (ASM), and the features that ASM inherits from Istio[1], a popular open source framework for creating, managing and implementing a service mesh.

Rgo maieptnimtenol lk c siecerv kyma rkn ngfk atitlsicfea tcnminoaounic etewben cvmoceirsirse gisnu s editedcda cmnioontcuami nametmnaeg crotnlo nelpa, jr acvf ulcidens sootl vr vrsebeo omiicctomunna tenbeew vsecs eri, zxz btrvibeiyoals, nanchee isrytuec, rootlcn pnloitcaiap aitcffr wvfl, zhn tlaeimus flastu nj zn anclioappti.