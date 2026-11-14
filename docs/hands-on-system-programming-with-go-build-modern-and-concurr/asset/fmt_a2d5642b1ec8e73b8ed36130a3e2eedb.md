
# Worker Pools and Semaphores in Go

## Worker Pool with Channels

A very typical concurrent approach is the workers pool. Here, you have a series of clients that can be picked up to process an input and different parts of the application can ask to use such clients, returning the clients back when they are done.

We can create more than one client that shares the same channels for both requests and responses, with requests being the coordinates and the results being the response from the service. Since the channel for responses is unique, we can define a custom type that holds all the information needed for that channel:

```
type result struct {
    Loc [2]float64
    Result []maps.Result
    Error error
}
```

The next step is creating the channels—we are going to read a comma-separated list of values from an environment variable here. We will create a channel for requests, and one for responses. Both channels have a capacity equal to the number of workers, in this case, but this would work even if the channels were unbuffered. Since we are just using channels, we will need another channel, `done`, which signals whether a worker has finished working on their last job:

```
keys := strings.Split(os.Getenv("MAPS_APIKEYS"), ",")
requests := make(chan [2]float64, len(keys))
results := make(chan result, len(keys))
done := make(chan struct{})
```

Now, we will create a goroutine for each of the `keys`, in which we define a `client` that feeds on the `requests` channel, executes the request, and sends the result to the dedicated channel. When the `requests` channel is closed, the goroutine will exit the range and send a message to the `done` channel, which is shown in the following code:

```
for i := range keys {
    go func(id int) {
        log.Printf("Starting worker %d with API key %q", id, keys[id])
        client := maps.NewClient(maps.DailyCap, keys[id])
        for j := range requests {
            var r = result{Loc: j}
            log.Printf("w[%d] working on %v", id, j)
            r.Result, r.Error = client.ReverseGeocode(j[0], j[1])
            results <- r
        }
        done <- struct{}{}
    }(i)
}
```

The locations can be sent to the `requests` channel sequentially in another goroutine:

```
go func() {
    for _, l := range [][2]float64{
        {40.4216448, -3.6904040},
        {40.4163111, -3.7047328},
        {40.4123388, -3.7096724},
        {40.4145150, -3.7064412},
    } {
        requests <- l
    }
    close(requests)
}()
```

We can keep count of the `done` signals we are receiving and close the `results` channel when all the workers are done:

```
go func() {
    count := 0
    for range done {
        if count++; count == len(keys) {
            break
        }
    }
    close(results)
}()
```

The channel is used to count how many workers are done, and once every one of them is done, it will close the `results` channel. This will allow us to just loop over it to get the result:

```
for r := range results {
    log.Printf("received %v", r)
}
```

Using a channel is just one of the ways to wait for all the goroutines to finish, and we will see more idiomatic ways of doing it in the next chapter with the `sync` package.

## Pool of Workers

A channel can be used as a pool of resources that allows us to request them on demand. In the following example, we will create a small application that will look up which addresses are valid in a network, using a third-party client from the `github.com/tatsushid/go-fastping` package.

The pool will have two methods, one for getting a new client and another to return the client back to the pool. The `Get` method will try to get an existing client from the channel or return a new one if this is not available. The `Put` method will try to put the client back in the channel, or discard it otherwise:

```
const wait = time.Millisecond * 250

type pingPool chan *fastping.Pinger

func (p pingPool) Get() *fastping.Pinger {
    select {
    case v := <-p:
        return v
    case <-time.After(wait):
        return fastping.NewPinger()
    }
}

func (p pingPool) Put(v *fastping.Pinger) {
    select {
    case p <- v:
    case <-time.After(wait):
    }
    return
}
```

The client will need to specify which network needs to be scanned, so it requires a list of available networks starting with the `net.Interfaces` function, ranging through the interfaces and their addresses:

```
ifaces, err := net.Interfaces()
if err != nil {
    return nil, err
}
for _, iface := range ifaces {
    // ...
    addrs, err := iface.Addrs()
    // ...
    for _, addr := range addrs {
        var ip net.IP
        switch v := addr.(type) {
        case *net.IPNet:
            ip = v.IP
        case *net.IPAddr:
            ip = v.IP
        }
        // ...
        if ip = ip.To4(); ip != nil {
            result = append(result, ip)
        }
    }
}
```

We can accept a command-line argument to select between interfaces, and we can show a list of interfaces to the user to select when the argument is either not present or wrong:

```
if len(os.Args) != 2 {
    help(ifaces)
}
i, err := strconv.Atoi(os.Args[1])
if err != nil {
    log.Fatalln(err)
}
if i < 0 || i > len(ifaces) {
    help(ifaces)
}
```

The `help` function is just a print of the interfaces IP:

```
func help(ifaces []net.IP) {
    log.Println("please specify a valid network interface number")
    for i, f := range ifaces {
        mask, _ := f.DefaultMask().Size()
        fmt.Printf("%d - %s/%v\n", i, f, mask)
    }
    os.Exit(0)
}
```

The next step is obtain the range of IPs that need to be checked:

```
m := ifaces[i].DefaultMask()
ip := ifaces[i].Mask(m)
log.Printf("Lookup in %s", ip)
```

Now that we have the IP, we can create a function to obtain other IPs in the same network. IPs in Go are a byte slice, so we will replace the least significant bits in order to obtain the final address. Since the IP is a slice, its value will be overwritten by each operation (slices are pointers). We are going to update a copy of the original IP—because slices are pointers to the same array—in order to avoid overwrites:

```
func makeIP(ip net.IP, i int) net.IP {
    addr := make(net.IP, len(ip))
    copy(addr, ip)
    b := new(big.Int)
    b.SetInt64(int64(i))
    v := b.Bytes()
    copy(addr[len(addr)-len(v):], v)
    return addr
}
```

Then, we will need one channel for results and another for keeping a track of the goroutines; and for each IP, we need to check whether we can launch a goroutine for each address. We will use a pool of 10 clients and inside each goroutine—we will ask for each client, then return them to the pool. All valid IPs will be sent through the `address` channel:

```
done := make(chan struct{})
address := make(chan net.IP)
ones, bits := m.Size()
pool := make(pingPool, 10)
for i := 0; i < 1<<(uint(bits-ones)); i++ {
    go func(i int) {
        p := pool.Get()
        defer func() {
            pool.Put(p)
            done <- struct{}{}
        }()
        p.AddIPAddr(&net.IPAddr{IP: makeIP(ip, i)})
        p.OnRecv = func(a *net.IPAddr, _ time.Duration) { address <- a.IP }
        p.Run()
    }(i)
}
```

Each time a routine finishes, we send a value in the `done` channel so we can keep count of the `done` signals received before exiting the application. This will be the result loop:

```
i = 0
for {
    select {
    case ip := <-address:
        log.Printf("Found %s", ip)
    case <-done:
        if i >= bits-ones {
            return
        }
        i++
    }
}
```

The loop will continue until the count from the channel reaches the number of goroutines. This concludes the more convoluted examples of the usage of channels and goroutines together.

## Semaphores
