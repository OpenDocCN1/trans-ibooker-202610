![](bbox=[0.39, 0.1, 0.65, 0.22])

Figure 9-14. mwc-textfield Component

Then run the following in your terminal:

```
$npm install --save @material/mwc-textfield
```

You can see all its properties at https://github.com/material-components/material-components-web-components/tree/master/packages/textfield.

Install mwc-top-app-bar (Figure 9-15).

![](bbox=[0.32, 0.43, 0.72, 0.47])

# Login

Figure 9-15. mwc-top-app-bar Component

Then run the following in your terminal:

```
$npm install --save @material/mwc-top-app-bar
```

You can see all its properties at https://github.com/material-components/material-components-web-components/tree/master/packages/top-app-bar.

Now we are going to install Polyfills to support older web browsers.

Install the following dependencies:

```
$npm install --save-dev copy-webpack-plugin @webcomponents/webcomponentsjs
```

Add copy-webpack-plugin to Vue's Webpack configuration file. To do this, we must create a new file, vue.config.js, and add the code in Listing 9-17.