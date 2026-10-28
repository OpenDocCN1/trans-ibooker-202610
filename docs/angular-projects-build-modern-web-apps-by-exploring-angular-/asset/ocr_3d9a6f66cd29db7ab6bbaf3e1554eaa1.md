+   4. Open the main entry point file of our application, main.ts, and add the following line:

```
// eslint-disable-next-line @typescript-eslint/dot-
// notation
window['CESIUM_BASE_URL'] = '/assets/cesium/';
```

The CESIUM_BASE_URL global variable indicates the location of the CesiumJS source files.

+   5. Install a custom webpack builder using the following npm command:

```
npm install -D @angular-builders/custom-webpack
```

A **builder** is an Angular library that extends the default functionality of the Angular CLI. The @angular-builders/custom-webpack builder allows us to provide an additional webpack configuration file while building our application. It is beneficial in cases where we want to include additional webpack plugins or override existing functionality.

+   6. Create a new webpack configuration file named extra-webpack.config.js in the root folder of the project and add the following content:

```
module.exports = {
  resolve: {
    fallback: {
      fs: "empty",
      Buffer: false,
      http: "empty",
      https: "empty",
      zlib: "empty"
    }
  },
  module: {
    unknownContextCritical: false
  }
};
```

The configuration file will ensure that webpack will not try to load CesiumJS code that cannot understand. CesiumJS uses modules in a format that cannot be statically analyzed from webpack.