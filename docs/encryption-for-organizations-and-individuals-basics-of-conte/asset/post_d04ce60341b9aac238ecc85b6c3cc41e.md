
Even large corporations can be guilty of installing rootkits on the devices of their clientele. The biggest such case was the copy-protection scandal of 2005 coming from *Sony BMG*. Upon inserting a CD into a computer, a rootkit was silently installed on the system, greatly reducing its level of security against actual malware as well as eavesdropping on the usage habits of the user. The corporation first utilized a copy-protection scheme known as *Extended Copy Protection (XCP)* and later one known as *MediaMax CD-3*; both of these were basically malicious software.

While originally intended as a type of efficient copy-protection software, Sony BMG’s rootkits compromised approximately 22 million users. This resulted in class-action lawsuits by litigators from the states of Texas, New York, and California.

## Spyware

Closely related to policeware, *spyware* is pretty much the same thing – except it’s usually coming from malicious, nongovernmental actors. Identity theft thrives from this type of malware; a person’s identity and credit card numbers are well sought after among cybercriminals. Spyware usually operates fully covertly on an unsuspecting user’s devices.

Credit card numbers aside, things that spyware can leak include usernames, passwords, financial records, and Internet browsing history. For purposes of blackmail, these types of malicious programs are often a cybercriminal’s first choice. According to the *Federal Trade Commission (FTC)*, even back in 2003 more than 27.3 million Americans had been victims of identity theft of which a considerable portion was facilitated by spyware.

## Virus

A virus is a type of self-replicating malware which can spread rapidly especially over online environments, potentially infecting tens of millions of computers and other devices. As of 2020 (and up until now), most viruses target Microsoft Windows operating systems. Since Windows is a very popular choice for organizations of all sizes, viruses cause financial losses of several billion dollars a year due to causing slowdowns, system crashes, and many other time-consuming scenarios. It’s to be noted viruses do not self-execute; a user has to activate them.

Computer viruses can be thought of as a form of semi-intelligent artificial life. The first software program that ticks most of the boxes for being a virus was The Creeper System written by software developer Bob Thomas in 1971. However, the program did very little damage, simply displaying the message “I’m the creeper: catch me if you can” on infected computers of the time.

Signs your system may be infected with a virus/malware include a more sluggish computer, the introduction of software you never installed on your desktop, a browser home page you can’t reset, and strange email sent from your email account(s). Other symptoms include annoying pop-ups and sudden shutdowns of your favorite programs.

## Worm

Closely related to a virus is a worm, which is simply not only self-replicating but also automatically spreading and self-executing type of malicious software. Worms often spread through email and don’t need any user interaction to activate themselves. They are among the most dangerous form of malware (see Table 5-2).

![](img/df9b6c40c665f58cab0c435444990c63_117_0.png)

Figure 5-2. A prompt from the notorious Cryptolocker ransomware

There are plenty of lower-tier ransomware programs out there which can be removed with some know-how, but more advanced varieties of ransomware are only theoretically disposed of. Never execute, or allow your colleagues to execute, programs you’re not absolutely comfortable with.

## In Closing

After finishing this chapter, you will have hopefully gained awareness of the following:

- The main four cryptographic attack models
- What linear and differential cryptanalysis refer to
- The basics of the most common attacks against cryptographic systems and which countermeasures work best against them
- What various types of malware there are and the basics of their operation

In the next chapter we’ll take a good gander at how to create near impenetrable contemporary systems using encryption and other approaches.

## CHAPTER 6: Creating Extremely Secure Encrypted Systems

Like you may remember, in Chapter 4 we took a quick glance at encryption solutions for three of the most popular operating systems as of 2020: Microsoft Windows, MacOS, and Linux. In this chapter we’ll not only expand on this topic, but also cover the best general security measures available for these operating systems in detail. We’ll tackle the best anti-malware and other related solutions of today, in both software and hardware. After all, there’s no effective cryptography without defensive technologies against every variety of serious malware. Feel free to implement as many of these countermeasures as you can.

Due to the possibility of backdoors, in this chapter we’ll also have a focus on open source solutions for all software presented. Closed-source encryption software will still be featured as it does have its uses, even when/if backdoored.

### On Multilayer Encryption

As it stands, the mighty *Advanced Encryption Algorithm (AES)* is pretty much invincible until the unforeseeable future. Using a single layer of AES should be enough for even the most fervent cryptographers. However, there is an ongoing debate over the use of multilayered encryption. This approach and its associated concepts will be discussed next.

On the surface additional layers of encryption seem like great countermeasures against cryptographic attacks. Now, there are two schools of thought on this. Some proponents of multilayer encryption rightfully claim that in theory each additional implementation of an encrypting algorithm fortifies a dataset considerably. Others feel each such implementation only adds attack vectors for malicious actors to utilize. In addition, using numerous encryption algorithms simultaneously creates more strain on a system’s resources, whether it seems negligible or not to involve human actors. Still, backdoors in closed-source commercial encryption products, such as Windows BitLocker, are a legitimate concern for many cryptographers.

Provided well-established algorithms such as AES are used, the biggest threat to a secure system lies in malicious software, often facilitated by human incompetence. This is why a large section of this chapter is dedicated to presenting the most effective countermeasures against malware known in 2020.

Whether one is suspicious of commercial encryption products or not, such a full-disk encryption is a great way to fend off data leaks obtained during burglaries and many forms of espionage. The government is unlikely to side with the offenders in these cases. Now, as demonstrated in Chapter 4, one solution against those pesky backdoors is to store your most delicate data within secure file containers. These should be naturally encrypted with open source solutions only, such as the much loved *VeraCrypt*. A double layer of, say, BitLocker and file containers consisting of another dose of 256-bit AES data is a tough nut to crack for any actor, whether highly skilled and malicious or not.

Now, should one implement multilayer encryption, some practices should be adhered to. For one, the passwords for each layer should be completely different and under no circumstance share patterns in any way. Also, weak algorithms for both encryption and hashing should be steered clear of. These include the *Digital Encryption Standard (DES)* in all of its varieties, as well as the *SHA-1* and *MD5*. The public-key mammoth that is the RSA algorithm, while not obsolete, doesn’t represent the cutting edge in encryption any longer; it has some issues in both security and performance. Some cryptographers suggest either using RSA at its higher key sizes of 2048 bits (or more) or abandoning it altogether. Remember, the cryptographic quantum revolution is right around the corner as of 2020. Its first casualty may very well be algorithms like the once-venerable RSA.

### Small vs. Big Business Readiness

While organizations of all sizes obviously need to implement proper security measures both online and in the traditional sense, there’s often a different approach to these matters when it comes to smaller and established businesses. Larger enterprises have much more resources to spare, while the smaller actors tend to prioritize growth instead of security. This chapter is primarily for these less established business owners/organizations, who may have not had the opportunity to outsource their cybersecurity solutions yet.
