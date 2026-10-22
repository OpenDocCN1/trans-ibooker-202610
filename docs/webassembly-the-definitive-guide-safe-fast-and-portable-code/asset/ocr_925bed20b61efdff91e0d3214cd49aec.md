```
console.log(image);
};
</script>
</body>
</html>
```

The result of calling our C++ application and rendering the results in a canvas after reading it back in via JavaScript can be seen in Figure 6-5.

![](bbox=[0.16, 0.26, 0.83, 0.61])

Figure 6-5. The result of rendering our Bitmap file in the canvas

I hope you are at least a little impressed. It is pretty cool how little we had to do to run this C++ code in the browser! There are still some issues with respect to performance and threading, but you have come a long way from adding two numbers together.

One thing that we could do is add a command line parameter to the execution to select which of the tests to run. For the time being, we are not going to worry about the tests that read in the sample image⁶.

To accept parameters on the command line, we need to modify the main() method to what you see in Example 6-13.