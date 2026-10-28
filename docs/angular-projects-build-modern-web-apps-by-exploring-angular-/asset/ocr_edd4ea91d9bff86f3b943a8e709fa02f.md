# Packaging a desktop application

Web applications are usually bundled and deployed to a web server that hosts them. On the other hand, desktop applications are bundled and packaged as a single executable file that can be easily distributed. Packaging our WYSIWYG application requires the following steps:

+   - Configuring webpack for production mode
+   - Using an Electron bundler

We will look at both of them in more detail in the following sections.

# Configuring webpack for production

We have already created a webpack configuration file for the development environment. We now need to create a new one for production. Both configuration files will share some functionality, so let's start by creating a common one:

+   1. Create a webpack.dev.config.js file in the root folder of the Angular CLI workspace with the following content:

```
const path = require('path');
const baseConfig = require('./webpack.config');

module.exports = {
  ...baseConfig,
  mode: 'development',
  devtool: 'source-map',
  output: {
    path: path.join(process.cwd(), 'dist', 'my-editor'),
    filename: 'shell.js'
  }
};
```

+   2. Remove the mode, devtool, and output properties from the webpack.config.js file.