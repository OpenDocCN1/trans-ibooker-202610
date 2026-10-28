
- Understand why LinkedIn is important for all professionals, especially cybersecurity professionals
- Publish your own professional profile on LinkedIn
- Actively use LinkedIn both to give and to get professional benefits

### Lab Materials and Setup

The materials you need for this lab are

- *Principles of Computer Security: CompTIA Security+ and Beyond* textbook
- A web browser with an Internet connection

### Let’s Do This!

On LinkedIn, you can

- Meet and network with professionals in any industry, sector, or field.
- Find a mentor and become a mentor.
- Get discovered by hiring managers and recruiters.
- Stay in contact with other professionals and share career milestones.
- Brand and promote yourself with a profile for professionals to see.
- Get validated by professionals, in terms of your knowledge and expertise.
- Share and view creative content and as a way to connect with others.
- Learn about companies, track companies for future job opportunities, and engage with companies.

And the biggest reason, in my opinion, for joining LinkedIn is to…

- Stay current with events and happenings in your industry, sector, or field.

![](img/6e921d997751f0a0f87e5170ac9657a1_26_0.png)

**Step 1** I post dozens of articles, most days, on technology, networking, systems administration, cybersecurity, hacking, pentesting, forensics, malware, cryptography, programming, and more. Reading, posting, and discussing these articles on LinkedIn with fellow professionals and students (I have over 27,000 connections) are some of the many ways I stay up to date on everything.

- a. Follow me on LinkedIn (See Figure 1-1), and join in on the fun!
  - https://linkedin.com/in/jonathan-s-weissman-058b649b
- b. Read the latest article I posted on LinkedIn.
- c. Reply to my post with your thoughts on the article.

Think about this. If someone reacts to, comments on, or shares your posts, their connections (two-way relationships) and followers (those who see your posts, articles, and shares) will see it—and you! When you react to or comment on my posts, my connections and followers will see you! These are great ways to grow your own network.

### Note

When you are connected to someone on LinkedIn, you are following them and they are following you by default.

![](img/6e921d997751f0a0f87e5170ac9657a1_28_0.png)

Figure 1-1 Jonathan S. Weissman on LinkedIn

Certifications are another way that I stay current. I stress the importance of industry certifications to my students. In that same vein, I model myself for my students. Having achieved 44 high-level industry certifications, I am never not studying for a certification exam. The first thing I do upon earning a new cert is to pick my next target. This shows my students that the learning process never ends, and, especially in this industry, if you stand still in terms of your knowledge or skills for even a short amount of time, you could be obsolete and undesirable rather quickly. By studying for the CompTIA Security+ certification, you’re following me on that path as well!

### Note

Other ways I stay current include consulting, appearing on TV news as a cybersecurity expert, inspiring minds at speaking engagements, writing articles/blogs, being quoted in articles/blogs of others, performing technical edits on industry books, and writing industry books like this one. Some or all of these could be good future goals for you!

**Step 2** A good link to check out is https://university.linkedin.com/linkedin-for-students, which features lots of resources that illustrate why all students should absolutely have a LinkedIn profile.

- a. Screenshot the most valuable resource you find at the LinkedIn for Students page.
- b. Explain why you think it is the most valuable resource.

30 MINUTES

## Lab Exercise 1.02: Cyber Threat Maps

Cyberattacks are happening every second of every minute of every hour of every day. No organization, industry, or part of the world will ever be immune to them.

One way to visualize these constant attacks is through cyber threat maps, also known as cyberattack maps. These maps enable you to “watch” attacks traversing countries and continents. Most of them look like video games or something out of Star Wars, with colorful beams of light representing attacks from one part of the world to another.

Networks of sensors, in some cases millions of them from cybersecurity companies, span the world and collect information on cyber threats. In most cases, these sensors are connected to honeypots, which are systems simulating applications, services, data, and devices that don’t represent real entities. The cybercriminals aren’t aware of this, though, and when they attack these honeypots, information about the cybercriminals, including their IP addresses and tools used, are logged and subsequently added to the maps. The IP addresses, though, are in most cases spoofed or represent infected.

Dynamic nature of the Internet, some of these links may be inactive by the time you see them. However, at the time of writing, these are my “top ten” cyber threat maps.

**Step 1** Check out the following cyber threat maps:

- **FireEye Cyber Threat Map**
  http://www.fireeye.com/cyber-map/threat-map.html
- **Threatbutt Internet Hacking Attribution Map**
  https://threatbutt.com/map
- **Check Point ThreatCloud Live Cyber Threat Map**
  https://threatmap.checkpoint.com/ThreatPortal/livemap.html (see Figure 1-2)

![](img/6e921d997751f0a0f87e5170ac9657a1_32_0.png)

Figure 1-2 Check Point’s ThreatCloud threat map

- Deteque Botnet Threat Map
  https://www.deteque.com/live-threat-map/
- Kaspersky Cyberthreat Real-Time Map
  https://cybermap.kaspersky.com/
- Digital Attack Map
  http://www.digitalattackmap.com
- Akamai Real-Time Web Attack Monitor
  https://www.akamai.com/us/en/resources/visualizing-akamai/enterprise-threat-monitor.jsp

- e. How frequently do you check on unknown links received in e-mail before clicking on them?
- f. How frequently do you click unknown links received in e-mail?
- g. How frequently do you check on unknown files received in e-mail before downloading them?
- h. How frequently do you download unknown files received in e-mail?
- i. How frequently do you use public Wi-Fi without a VPN (virtual private network)?
- j. How frequently do you think that you let your guard down with regard to cybersecurity?

From a cybersecurity hygiene best practice, here are the responses you hope to receive in your survey:

- a. Always
- b. Often
- c. Often
- d. Always
- e. Always
- f. Never
- g. Always
- h. Never
- i. Never
- j. Rarely

**Step 2** Send your survey to as many friends and relatives as possible. Try to get responses from at least 30 people.

**Step 3** Read How To Use Google Slides at https://support.google.com/docs/answer/2763168. Then create a presentation at https://docs.google.com/presentation/ analyzing and critiquing the results.

120 MINUTES

## Lab Exercise 1.04: Building the Virtual Lab

You’re going to need some other machines to do the hands-on labs in this book. How about using a virtual machine (VM) instead of a physical machine for each machine needed? You’ll download Offensive Security’s Kali Linux, the most used pentesting/ethical hacking Linux distribution. From the Microsoft Evaluation Center, you’ll download 90-day trial versions of Windows 10 and Windows Server 2019. Information about extending the trials is included in this lab exercise. To run the virtual machines, you’ll download, install, and run VMware Workstation Player, one of VMware’s many hypervisors.

### Learning Objectives

In this lab exercise, you’ll download and install a hypervisor and create virtual machines for multiple guest operating systems. At the end of this lab exercise, you’ll be able to

- Perform the hands-on labs in future chapters using multiple virtual machines

### Lab Materials and Setup

The materials you need for this lab are

- *Principles of Computer Security: CompTIA Security+ and Beyond* textbook
- A web browser with an Internet connection
- A Windows 10 machine to which you have administrative privileges (desktop or laptop)

### Let’s Do This!

You’ll be installing and using VMware Workstation Player, Kali Linux, Windows 10 Enterprise, and Windows Server 2019 in future labs in this
