6. Add statements to get the IP address for the entered website, as shown in the following code:

```
IPHostEntry entry = Dns.GetHostEntry(uri.Host);
WriteLine($"{entry.HostName} has the following IP addresses:");
foreach (IPAddress address in entry.AddressList)
{
    WriteLine($"   {address} ({address.AddressFamily})");
}
```

7. Run the code, enter a valid website address or press ENTER, and view the result, as shown in the following output:

```
stackoverflow.com has the following IP addresses:
    151.101.193.69 (InterNetwork)
    151.101.129.69 (InterNetwork)
    151.101.1.69 (InterNetwork)
    151.101.65.69 (InterNetwork)
```

# Pinging a server

Now you will add code to ping a web server to check its health:

1. Import the namespace to get more information about networks, as shown in the following code:

```
using System.Net.NetworkInformation; // Ping, PingReply, IPStatus
```

2. Add statements to ping the entered website, as shown in the following code:

```
try
{
    Ping ping = new();
    WriteLine("Pinging server. Please wait...");
    PingReply reply = ping.Send(uri.Host);

    WriteLine($"{uri.Host} was pinged and replied: {reply.Status}.");
    if (reply.Status == IPStatus.Success)
    {
        WriteLine("Reply from {0} took {1:N0}ms",
            arg0: reply.Address,
            arg1: reply.RoundtripTime);
    }
}
catch (Exception ex)
{
    WriteLine($"{ex.GetType().ToString()} says {ex.Message}");
}
```