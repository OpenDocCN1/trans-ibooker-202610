```
$ docker run -p 4222:4222 -p 8222:8222 -v ~/tmp/jetstream:/tmp/jetstream nats -js -m 8222
[1] 2021/06/26 21:23:21.572029 [INF] Starting nats-server
[1] 2021/06/26 21:23:21.572258 [INF] Version: 2.3.0
[1] 2021/06/26 21:23:21.572289 [INF] Git: [56a144a]
[1] 2021/06/26 21:23:21.572311 [INF] Name: NDT... 
[1] 2021/06/26 21:23:21.572378 [INF] Node: N3Qm3ud7
[1] 2021/06/26 21:23:21.572399 [INF] ID: NDT... 
[1] 2021/06/26 21:23:21.574923 [INF] Starting JetStream
[1] 2021/06/26 21:23:21.576592 [INF] 
[1] 2021/06/26 21:23:21.576622 [INF] JETSTREAM
[1] 2021/06/26 21:23:21.576631 [INF] 
[1] 2021/06/26 21:23:21.576639 [INF] 
[1] 2021/06/26 21:23:21.576647 [INF] 
[1] 2021/06/26 21:23:21.576656 [INF] https://docs.nats.io/jetstream
[1] 2021/06/26 21:23:21.576664 [INF] ---------------- JETSTREAM ----------------
[1] 2021/06/26 21:23:21.576697 [INF] Max Memory: 2.06 GB
[1] 2021/06/26 21:23:21.576711 [INF] Max Storage: 4.64 GB
[1] 2021/06/26 21:23:21.576779 [INF] Store Directory: "/tmp/nats/jetstream"
[1] 2021/06/26 21:23:21.576800 [INF] --------------------------------------------
[1] 2021/06/26 21:23:21.579799 [INF] Starting http monitor on 0.0.0.0:8222
[1] 2021/06/26 21:23:21.580581 [INF] Listening for client connections on 0.0.0.0:4222
[1] 2021/06/26 21:23:21.581147 [INF] Server is ready
```

We can pass the full command-line arguments. In this case, we are specifying a volume to be mounted in our ~/tmp directory to be used for JetStream persistence.

# Monitoring NATS servers

NATS exposes a monitoring endpoint that can be viewed through the browser, the command line, or a web browser, as shown in Figure 10-9.

![](bbox=[0.34, 0.49, 0.66, 0.75])

Figure 10-9. The NATS monitoring endpoint

The following is an example of the output of the NATS monitoring endpoint:

```
$ curl http://100.119.182.45:8222/varz
{
  "server_id": "ND6FZROVHED32BDLBTMCPXEN4L1KB2ROR3HO3GZ6DLDB64Z54SLG5GOD",
  "server_name": "ND6FZROVHED32BDLBTMCPXEN4L1KB2ROR3HO3GZ6DLDB64Z54SLG5GOD",
  "version": "2.2.2",
  "proto": 1,
```