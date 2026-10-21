+   - Maintain these components in the long run, doing backups and making sure that the fail-over mechanisms are working as expected.

![](bbox=[0.08, 0.26, 0.92, 0.64])

Figure 4.2 Provisioning a new PostgreSQL instance using the PostgreSQL Helm Chart

If you are working with Helm Charts, there are a couple of caveats and tricks that you need to be aware of:

+   - If the chart doesn’t allow you to configure a parameter that you are interested in changing you can always use `helm template`, then modify the output to add or change the parameters that you need to finally install the components using `kubectl apply -f`. Alternatively, you can submit a Pull Request to the chart repository. It is a common practice to not expose all possible parameters and wait for community members to suggest more parameters to be exposed by the chart. Don’t be shy and get in touch with the maintainers if that is the case. Whatever modification that you do the chart content will need to be maintained and documented. By using `helm template` you lose the Helm release management features, which allows you to upgrade a chart when a new chart version is available.