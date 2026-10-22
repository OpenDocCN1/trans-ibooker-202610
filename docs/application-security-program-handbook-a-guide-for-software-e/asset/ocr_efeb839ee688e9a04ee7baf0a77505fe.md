![](bbox=[0.17, 0.18, 0.92, 0.32])

Figure 3.1 Threat modeling in the Secure SDLC

# 3.1.1 Basic threat modeling terminology

Before we get into actually performing a threat model you need to know a few terms that are used during the process.

+   - Attacker - Those who would misuse intentionally or unintentionally an element of the system under consideration. This could be one of the threat actors I spoke of earlier like a script kiddie, hacktivist, or others.
+   - Asset – Anything you deem to have value and something that the system must protect from an attacker. Some physical examples are money and precious metals. Digital examples are data, especially sensitive data like protected health information, or personally identifiable information.
+   - Threat - A means by which an attacker might compromise an asset that has potential for success. Threats can include everything from hackers and malware, from earthquakes to wars. Additionally, intention is not a factor when considering threats. A mechanical failure of a hard drive in a data center is an equal threat to a coordinated attack by an attacker.
+   - Risk – The potential for loss, damage, or destruction of an asset as a result of a threat exploiting a vulnerability. In the example previously about the mechanical failure of a hard drive and an attack by an attacker, the mechanical failure is lower overall risk since there is usually redundancy built into an architecture to manage a failure meaning the impact is smaller.

When you want to understand risk and treats, it’s important to ensure that you take emotion and gut feeling out of the equation. Take the two visuals in figure 3.2. Many people have a very visceral reaction to the bear and view the stairs as just another daily activity, but what are the actual risks?