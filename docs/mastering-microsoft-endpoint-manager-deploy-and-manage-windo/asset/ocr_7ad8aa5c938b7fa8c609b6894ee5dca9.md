# Supported endpoints – device redirection settings

In terms of needing to support a camera or serial port redirection, this isn't default-enabled as part of the Windows 365 baseline configuration. Out of the box, Windows 365 allows you to redirect local audio, your clipboard, and your microphone.

To allow for more redirection features, you must allocate different configuration sets from within Microsoft Endpoint Manager's Intune configuration profiles. The reason why we left so many settings disabled is due to security. Our service aligns with our zero-trust principles, so we left it disabled by default to let the customers decide what best fits their use case.

By default, end users will have a larger set of features, and admins can choose whether to allow all these redirections or to create policies to block them.

You can enable these settings like so:

![](bbox=[0.118, 0.387, 0.85, 0.647])

# Information Worker Portal (IWP)

As explained in the Supported endpoints section, end users can access their cloud PC from different platforms. The web portal is the most unified method as it only requires an HTML5 supported browser such as Microsoft Edge.

To simplify access, we created the IWP, which you can see in the following screenshot. The portal allows end users to choose between accessing their cloud PC via the browser or the Remote Desktop (MSRDC) client.