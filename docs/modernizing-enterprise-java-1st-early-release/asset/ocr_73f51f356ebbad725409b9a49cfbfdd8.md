```
```text
 __  __   ___  ___  ____  ____  _   _  ____  ____
|  \/  | / _ \/ _ \/ ___||  _ \| | | |/ ___||  _ \
| |\/| || | | | | | \___ \| |_) | | | |\___ \| | | |
| |  | || |_| | |_| |___) |  __/| |_| | ___) | |_| |
|_|  |_| \___/\___/|____/|_|    \___/ |____/|____/

2021-06-25 16:51:24,023 INFO [io.quarkus] (main) function 1.0.0-SNAPSHOT on JVM (powered by Quarkus 1.13.0.Final) started in 1.399s. Listening on: http://0.0.0.0:8080
2021-06-25 16:51:24,027 INFO [io.quarkus] (main) Profile prod activated.
2021-06-25 16:51:24,028 INFO [io.quarkus] (main) Installed features: [cdi, funqy-knative-events]
```
```

In another terminal, verify the process is running:

```
```bash
$ docker ps | grep modernizingjavaappsbook/quarkus-faas:latest
cd1dd0ccc9b2  modernizingjavaappsbook/quarkus-faas:latest
"/cnb/process/web"      3 minutes ago   Up 3 minutes   5005/tcp,
127.0.0.1:8080->8080/tcp
    musing_carson
```
```

Try to access it:

```
```bash
$ curl \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"message":"Hello FaaS!"}' \
  http://localhost:8080
```
```

You should get an output similar to this:

```
```json
{"message":"Hello FaaS!"}
```
```

Now let’s deploy it to Kubernetes and let Knative use it as a scale-to-zero application. When we invoke the function via HTTP, Knative will start it automatically, and it will scale down to zero when not used.

```
```bash
$ func deploy
```
```

After a few seconds, you should see an output similar to this: