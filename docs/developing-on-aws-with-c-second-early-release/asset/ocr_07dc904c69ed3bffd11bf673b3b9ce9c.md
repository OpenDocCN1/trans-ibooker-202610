including all the permissions needed. You can view and edit the permissions for this (or any other) IAM role by visiting the IAM Dashboard.

Figure 3-2 shows our example function in the AWS Management Console for the region we deployed to. You can test the function directly in the console from the “Test” tab on the next screen.

![](bbox=[0.145, 0.235, 0.855, 0.455])

Figure 3-2. Function list in the AWS Lambda Console

This test window (shown in Figure 3-3) allows you to enter some JSON to pass into your function in the input parameter we defined earlier. Since we declared our input value as being of type string in

```
FunctionHandler(string input, ILambdaContext context)
```

we should change this to be any string and we can test the function directly in the management console.