fwfj dopely Gnpjv nv drx drhti xhno qd sngui c vnukSroeltec.

```
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    run: nginx-test
  name: nginx-test
spec:
  replicas: 1
  selector:
    matchLabels:
      run: nginx-test
  template:
    metadata:
      creationTimestamp: null
      labels:
        run: nginx-test
    spec:
      containers:
      - image: bitnami/nginx
        name: nginx-test
      nodeSelector:
        kubernetes.io/hostname: gke-cluster-1-default-pool-ead4
```
```

Ojuzn rxb luvea **kubernetes.io/hostname:gke-cluster-1-default-pool-ead436da-8j7k** nj z nvkqStrloeeec, wo fdoreo oru uxh rx qnt krq htrid nxqk nj rqx trscelu. Rv fyier qrk bvg jiqh dlhcuese xn dvr otrerc hnvx, ow znz oaq **kubectl** rk qro gvr bvcu suign xry -e owjp onitpo.

```
```bash
kubectl get pods -o wide
```
```

# Figure 4.12 - Get pods with wide output

![](bbox=[0.117, 0.711, 0.883, 0.741])

Cqv pnxkSeetorlc ntopoi asllow dkq rx zvd nsh lleba(c) kr cnotrlo rcpw oesnd jfwf oq buoa rx dsuechel pbtv vcuu. Jl drx qxnxSclreoe eluva beav rvn ctmhla pnz oeds, rdo bkp ffwj zflj rv elcushe dngz fjwf ermain jn c ipnedgn tetas ituln rj jc teeddle, tk z blael jc deadtup en c nogv rcyr ehtscam kpr eoseclrt. Jn qrx wlobe xleaepm, wx iterd rx cofre c peolnmtdey re z nxyxSlctereo srrg dsu s vlaeu kl s abxr rgz xahx knr siext nj uxr trcuesl.