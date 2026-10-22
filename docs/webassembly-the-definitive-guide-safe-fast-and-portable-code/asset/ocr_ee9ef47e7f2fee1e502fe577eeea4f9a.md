![](bbox=[0.17, 0.105, 0.83, 0.492])

Figure 17-1. TinyGo Playground in the browser

If you clone the TinyGo repo³, there are some examples that highlight interactions that should seem structurally familiar now even if you do not know Go.

If you install the compiler as detailed in Appendix A, you can run the examples. In Example 17-1 you can see the main.go file from examples/wasm/main.

# Example 17-1. Basic TinyGo example

```
package main

func main() {
    println("Hello world!")
}
```

To run the example, you have to execute the following. It builds the named example (e.g. main) and then copies the necessary files into an html