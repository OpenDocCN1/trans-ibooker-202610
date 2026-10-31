As this command runs, you will be asked to input required information, such as the AD SafeMode password and administrative credentials with permission to create a new domain controller:

![](bbox=[0.13, 0.145, 0.87, 0.455])

Figure 10.33: Creating your first Server Core DC

Once finished, the server will restart automatically and your new Server Core is now a headless domain controller in your environment!

# Verify that it worked

I know you trust me implicitly, but just for the sake of argument, what if we wanted to verify that those simple PowerShell commands really did what they said they did? Let's verify that DC3 really is a domain controller through a couple of different methods.