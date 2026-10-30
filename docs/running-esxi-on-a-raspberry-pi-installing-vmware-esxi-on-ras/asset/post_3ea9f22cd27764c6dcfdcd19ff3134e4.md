
VDI became a very attractive option in this situation, and fortunately many of the obstacles that remote home access had early on (namely high-speed, reliable Internet) were removed.

## Manageability

Maintaining and managing desktops has always been a painful proposition, and with VDI this aspect is greatly simplified. If a user is having an issue, for example, a technician can either shadow a user’s session or log in directly to the desktop. When it comes time to install new applications, patch the system, or otherwise maintain the software, the technician can do all of this directly. In many cases, virtual desktops are cloned from a gold image, and only the gold image needs to be updated and then pushed out.

## Data Locality

Apart from the more obvious benefits of VDI like security and manageability, one of the often-overlooked benefits of virtual desktops is data locality, that is, being as close to the data that needs to be processed as possible. Although data locality will never be an issue for a large portion of VDI users who only deal with common office documents that are quite small, there are cases where being close to the data can greatly affect a worker’s productivity, such as when working with large databases, videos, high-resolution images, and CAD models that are usually stored on a discrete storage appliance.

For example, if you need to edit video files that are stored on a company’s central storage repository, but you are located a couple of miles away, you could use a VPN to connect to this central storage from your home laptop (see Figure 11-3). Depending on how much data you are working with, it may become an exercise in frustration as the application may buffer the data due to the lack of bandwidth needed. Furthermore, if you have any jitter in your network, packets may need to be retransmitted.

![](img/430161835ae267467687add4a1cdc429_265_0.png)

*Figure 11-3. Streaming Data*

A local system using remote storage data could end up consuming the entire bandwidth of your public Internet connection, making it impossible for others to use it.

Figure 11-4 provides an example of how you could transfer the files from a remote location to a local system. Depending on the size of the files, you may not be able to store them locally, or your local storage system may not be able to deliver the performance needed.

![](img/430161835ae267467687add4a1cdc429_265_1.png)

*Figure 11-4. Transferring Files*

File security (as discussed previously) may also be an issue. If multiple people are working on the same files, trying to keep track of them can be infuriating, and if a file gets accidentally deleted, it may take a considerable amount of time to recreate it. By connecting to a virtual desktop that is located close to the data, however, you can minimize network traffic as the remote protocol only uses a fraction of the bandwidth compared to streaming the storage (see Figure 11-5).

![](img/430161835ae267467687add4a1cdc429_266_0.png)

*Figure 11-5. Using a Virtual Desktop to Edit the Files*

Some VDI protocols allow multiple users to interact with the same virtual desktop (see Figure 11-6). This can save countless hours and avoid the frustration of trying to describe what others are seeing and trying to accomplish. VMware has implemented this in Blast with its session collaboration ability.

![](img/430161835ae267467687add4a1cdc429_267_0.png)

*Figure 11-6. Collaboration*

In sum, when discussing the benefits of VDI, side benefits such as data locality should be considered along with the more obvious benefits like security, cost containment, and manageability.

## Types of Virtual Desktop Users

Table 11-1 shows the classification of virtual desktop users. This classification helps to better identify the resources that the virtual desktops need based on the type of workers that use them.

**Table 11-1. User Types**

| Classification | Workload | Resources Needed |
|---|---|---|
| Task Worker | Use one or two applications and are usually based in call centers. | Minimal desktop with one or two vCPUs, 2GB of RAM, and 32GB of storage. Companies may use Linux desktops to minimize costs. |
| Knowledge Worker | Use many different office applications as well as specialized applications. These are general office workers and corporate employees. | Full Windows desktop with a minimum of two vCPUs, 4GB of RAM, and storage appropriate for their use cases. |
| Power User | Engineers, content creators, and IT workers who use common office applications as well as specialized applications such as CAD/CAM, compilers, and other powerful applications. | Full Windows or Linux with a minimum of four vCPUs, 8GB of RAM, and storage appropriate for their particular use case. |

## Accessing Virtual Desktops

It’s possible to access a virtual desktop from any device that supports the protocol that the virtual desktop uses. The following are some of the more popular types of clients that have been developed to connect to virtual desktops.

### Media Redirection

Before discussing the different types of VDI clients, we need to first address a technology that is starting to come into play in the VDI world: media redirection. This is a technology that detects when certain applications such as web browsers, video conferencing, and other communication applications are redirected to run natively on the device where the VDI client is running rather than on the virtual desktop; that is, they are instantiated from the virtual desktop but run on the device itself. This is transparent to the user.

Figure 11-7 shows how, without redirection, browsing a website requires the application (in this case, a video website) to go from the website to the virtual desktop and then to the client. This introduces latency into the transaction and consumes CPU resources on the host on which the virtual desktop resides.

![](img/430161835ae267467687add4a1cdc429_269_0.png)

*Figure 11-7. Without Redirection*

Figure 11-8 shows how, by using redirection, a website can be accessed directly from the VDI client’s hardware, thereby cutting down the network traffic, latency, and CPU usage on the virtual desktop and using the excess hardware capacity of modern VDI clients.

![](img/430161835ae267467687add4a1cdc429_270_0.png)

*Figure 11-8. With Redirection*

Figure 11-9 shows the output of Top (a process monitoring application). This shows that Zoom, a popular video conferencing application, is running on and consuming the resources of the VDI client after being instantiated and redirected from the virtual desktop.

![](img/430161835ae267467687add4a1cdc429_270_1.png)

*Figure 11-9. Redirected Application Running on VDI Client*

### VDI Clients

Different classifications of clients have evolved over the years. Unfortunately, there isn’t a strict standard for the names of these classifications, and it is up to the vendor to self-describe the type of client they are selling. When discussing clients, I tend to use the term *VDI client* and then spell out the characteristics of the client, and the following are some examples. It’s worth noting, however, there is a lot of fluidity between what these clients provide.

- **Zero Clients**: A *zero client* is a device that has been purpose-built using the minimal amount of hardware required to connect to a virtual desktop. As these devices only run firmware, they are considered to be the most secure of all the types of clients but may not be as flexible as other client types.

- **Thin Clients**: *Thin clients* have been specifically designed to run as VDI clients. They usually have a low-powered and lower-cost processor and a minimal amount of RAM. They also only have enough storage to run an OS that has been designed to support VDI client software. Furthermore, because they run an OS rather than directly from firmware, they are easier to modify and may offer additional functionality than zero clients.

- **Thick Clients**: *Thick clients*, or fat clients, have more powerful hardware than zero or thin clients and are purpose-built by vendors as repurposed PCs or laptops with an application installed on them that allows them to connect to a virtual desktop. Whereas thin or zero clients are very dependent on virtual
