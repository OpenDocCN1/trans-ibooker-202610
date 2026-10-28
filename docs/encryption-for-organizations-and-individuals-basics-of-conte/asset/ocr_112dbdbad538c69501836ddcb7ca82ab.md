# Side Channel Attack (SCA)

A side channel attack doesn’t utilize any weaknesses in a system, rather it focuses on its actual implementation and what attack vectors that offers. There are several categories of side channel attacks including those based on timing of data transfer, data remanence, and electromagnetic leakage (see Table 5-1).

In theory, a successful side channel attack can bypass encryption of any strength as no cracking of the cryptographic scheme has to be attempted. It’s a scary thought. Luckily effective countermeasures exist.

# Table 5-1. Some typical cryptographic side channel attacks

| Type | Area of Focus | Skill Level Required |
|---|---|---|
| Acoustic | Audio (e.g., user keystrokes) | High |
| Data remanence | Critical data left on storage media (e.g., hard drives) | Low |
| Electromagnetic (EMF) | EMF emissions from computer monitors | High |
| Optical | Visual sightings of target system components, advanced photon analysis | Low/high |
| Power analysis | Changes in a device’s electricity usage | High |
| Timing | A system’s internal data transfers | High |

# Countermeasures Against Side Channel Attacks (SCAs):