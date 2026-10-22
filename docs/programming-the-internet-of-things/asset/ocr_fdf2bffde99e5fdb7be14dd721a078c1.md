# WARNING

There are lots of great 3rd party Jenkins plugins, but some are not well maintained and may introduce security vulnerabilities. If it’s a plugin you’re certain you need, be sure to track how often it’s updated and patched. If it’s not regularly maintained, it could introduce compatibility and security issues to your environment.

Once installed and secured, you can configure Jenkins to automatically monitor your Git repository locally or remotely, and run a build / test / deploy / run workflow on your local system, checking the success at each step. If, for example, the build fails because of a compile error in your code, Jenkins will report on this and stop the process. The same is true if the build succeeds, but the tests fail - the process stops at the first failure point. This ensures your local deployment won’t get overwritten with an update that doesn’t compile or fails to successfully execute the configured tests.

Setting up any local automation tool can be a complicated and time consuming endeavor. It’s super helpful, however, as it basically automates all the stuff you’re going to do to build, test and deploy your software. That said, it’s not required for any of the exercises in this book.

I’ve detailed my own approach using GitHub actions and self-hosted runners as well as Jenkins in a document on my website at https://programmingtheiot.com/setup/ConnectedDevices_DevE