```
import * as Module from './a.out.wasm';

console.log('The sum of 6 and 2 is: ' + Module.add(6,2));
```

Depending when you attempt to run the following, you may need the experimental feature flags, but notice how much easier it is to deal with than what we have seen before. The expression of the behavior is also dramatically more simple than we saw in the previous discussion on native addons. You can see why this community is excited about continued WebAssembly support being added over time.

```
brian@tweezer ~/g/w/s/c/node> node --experimental-modules ↵
  --experimental-wasm-modules index.mjs
(node:74571) ExperimentalWarning: Importing Web Assembly modules
is an
experimental feature. This feature could change at any time (Use
`node
--trace-warnings ...` to show where the warning was created)
The sum of 6 and 2 is: 8
```

As a final example, I want to pull in a more sophisticated third party library. For reasons that I will explain at the end of this chapter, finding a good example that did not open up too many cans of worms was tricky. There are things we have not introduced yet but we are starting to lay the foundation for in this chapter.

# Supply Chain Attacks

This brings us to another consideration. We are facing a very serious problem in the world of secure software systems called supply chain attacks. They are not a new problem, but they are getting worse and more frequent.

There is no single way to build a secure system, certainly not simply by turning on encryption or some security feature such as that. Those may be necessary for a secure system, but are most definitely insufficient. Usually through the combination of Defense in Depth⁴, the Principle of Least