BitLocker automatic device encryption uses BitLocker drive encryption technology to automatically encrypt internal drives after the user completes the OOBE on *Modern Standby* or **Hardware Security Testability Specification (HSTI)**-compliant hardware.

In the case of Modern Standby requirements or HSTI validation, this requirement is met by one of the following:

+   - Modern Standby requirements are implemented. These include requirements for **Unified Extensible Firmware Interface (UEFI)** Secure Boot and protection from unauthorized **Direct Memory Access (DMA)**.
+   - Starting with *Windows 10 version 1703*, this requirement can be met through an HSTI test.
+   - The Platform Secure Boot self-test (or additional self-tests as configured in the registry) must be reported by the HSTI as implemented and passed.
+   - Excluding Thunderbolt, the HSTI must not report any non-allowed DMA buses.
+   - If Thunderbolt is present, the HSTI must report that Thunderbolt is configured securely (the security level must be SL1 – *User Authorization* or higher).

*Windows 10 (Modern Standby)* expands the *Windows 8.1 Connected Standby power model*. Connected Standby, and consequently Modern Standby, enable an instant on/instant off user experience, like smartphone power models. Just like a phone, the S0 low-power idle model enables the system to stay connected to the network while in a low-power mode.

By leveraging the `powercfg /a` command, you can see whether your device supports Modern Standby (S0 Low Power Idle):

![](bbox=[0.255, 0.6, 0.71, 0.87])

Figure 7.19 – powercfg