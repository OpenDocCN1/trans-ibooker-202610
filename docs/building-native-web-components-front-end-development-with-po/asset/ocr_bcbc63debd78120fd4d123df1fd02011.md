![](bbox=[0.07, 0.1, 0.89, 0.27])

# Figure 8-1. Hello, World! example

You can run ESLint by executing $npm run lint.
The terminal will show all the code styling problems that are found in your component (see Figure 8-2).

```
> lit-element-starter-js@0.0.0 lint /Users/carlosrojaso/Projects/lit-element-starter-js
> lit-analyzer my-element.js && eslint '**/*.js'

Analyzing 1 files...

✓ Found 0 problems in 1 file
```

# Figure 8-2. Running ESLint

You can run tests with Karma, Chai, and Mocha by executing $ npm run test.
The terminal will show all the tests that we write for the component (see Figure 8-3).

```
> karma start karma.conf.cjs

START:
26 07 2020 16:23:17.024:WARN [filelist]: Pattern "/Users/carlosrojaso/Projects/lit-element-starter-js/_snapshots_/** /*.md" does not match any file.
26 07 2020 16:23:17.073:INFO [karma-server]: Karma v4.4.1 server started at http://0.0.0.0:9876/
26 07 2020 16:23:17.073:INFO [launcher]: Launching browsers ChromeHeadlessNoSandbox with concurrency unlimited
26 07 2020 16:23:17.083:INFO [launcher]: Starting browser ChromeHeadless
26 07 2020 16:23:17.593:INFO [HeadlessChrome 84.0.4147 (Mac OS X 10.15.6)]: Connected on socket iZiRPsCAH4c09V-AAAA with id 22384050
  my-element
    ✓ is defined
    ✓ renders with default values
    ✓ renders with a set name
    ✓ handles a click

Finished in 0.046 secs / 0.014 secs @ 16:23:18 GMT-0500 (Colombia Standard Time)

SUMMARY:
✓ 4 tests completed
```

# Figure 8-3. Running tests