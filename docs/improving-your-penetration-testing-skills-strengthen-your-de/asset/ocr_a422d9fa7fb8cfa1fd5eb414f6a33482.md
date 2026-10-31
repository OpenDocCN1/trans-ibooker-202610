6. Besides regular commands, Meterpreter also has support to process <ruby> directives, meaning that we can use all the power of Ruby in a resource script:

```
cat ~/.msf4/scripts/resource/meterpreter/ruby.rc
<ruby>
$stderr.puts("Ruby is awesome!")
$stderr.puts("session.platform: #{session.platform}, framework: #{framework}")
</ruby>
root@kali:~#
```

7. In this example, we are just printing the platform the session is running on as well as the framework, but you can imagine all the possibilities:

```
meterpreter > resource ruby.rc
[*] Processing /root/.msf4/scripts/resource
/meterpreter/ruby.rc for ERB directives.
[*] resource (/root/.msf4/scripts/resource
/meterpreter/ruby.rc)> Ruby Code (112 bytes)
Ruby is awesome!
session.platform: windows, framework:
#<Msf::Framework:0x005555e533c4b8>
meterpreter >
```

# Meterpreter timeout control

Meterpreter timeout control allows us to control the timeout behavior in Meterpreter sessions. Controlling timeouts allows us to change the noise level and other communication features, such as the duration of the Meterpreter session.

# How to do it...

+   1. The get_timeouts Meterpreter command displays the current timeout configuration:

```
meterpreter > get_timeouts
Session Expiry : @ 2017-11-19 05:59:46
Comm Timeout : 300 seconds
Retry Total Time: 3600 seconds
Retry Wait Time : 10 seconds
```