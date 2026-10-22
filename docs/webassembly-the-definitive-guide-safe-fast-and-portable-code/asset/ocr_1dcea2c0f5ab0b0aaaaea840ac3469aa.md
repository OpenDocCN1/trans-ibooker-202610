![](bbox=[0.18, 0.11, 0.82, 0.48])

Figure 11-1. Running a WASI application in a polyfill in a browser

WebAssembly makes your code portable. WASI endeavors to make your application portable by satisfying its expectations when you want it to.

# Capabilities-based Security

There is another aspect to WASI that is crucial to the future of WebAssembly in this ubiquitous computational landscape. We want to be able to provide functionality to the applications we want to provide them to, but not to malicious software. As we often cannot tell which is which, we will probably have limited sandbox environments as the default.

From its inception, WASI has been envisioned as a means of enforcing capabilities-based 10 security restrictions on the behavior it affords to hosted modules.