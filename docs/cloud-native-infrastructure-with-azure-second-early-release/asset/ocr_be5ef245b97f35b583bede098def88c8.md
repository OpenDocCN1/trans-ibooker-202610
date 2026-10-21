```
--set azure.clientID=$AZURE_CLIENT_ID \
--set azure.clientSecret=$AZURE_CLIENT_SECRET \
--set tunnelDisabled \
--set ipam.mode=azure \
--set masquerade=false \
--set nodeinit.enabled=true
--set kubeProxyReplacement=strict \
--set hostFirewall=true \
--set loadBalancer.algorithm=maglev \
--set k8sServiceHost=REPLACE_WITH_API_SERVER_IP \
--set k8sServicePort=REPLACE_WITH_API_SERVER_PORT
```

+   - We can verify the installation by checking that the four initial Pods have been created and that both a `cilium` and a `cilium-operator` Pod are running:

```
$ kubectl -n kube-system get pods --watch
cilium-operator-ad4375ds5-2x2q3        1/1     Running
0               4m18s
cilium-s5x8xk                    1/1     Running
0               4m19s
```

# Installing Hubble

You will also want to install Hubble, a UI that allows you to see a service map which depicts dependencies between services and statistics on network usage and performance. Hubble’s live-updating, graph-like interface (see Figure 8-6) allows you to present a lot of information about the layout and flow of data between containers. Furthermore, it provides an interface to model Cilium policy.