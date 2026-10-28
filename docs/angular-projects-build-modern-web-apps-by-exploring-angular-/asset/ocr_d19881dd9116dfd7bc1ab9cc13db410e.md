The original application bundle is now generated into a browser folder, inside the standard output folder:

```
"options": {
  "outputPath": "dist/gh-portfolio/browser",
  "index": "src/index.html",
  "main": "src/main.ts",
  "polyfills": "src/polyfills.ts",
  "tsConfig": "tsconfig.app.json",
  "inlineStyleLanguage": "scss",
  "assets": [
    "src/favicon.ico",
    "src/assets"
  ],
  "styles": [
    "src/styles.scss"
  ],
  "scripts": []
}
```

Thus, an Angular Universal application generates two versions of the same Angular application, one version for the server and another one for the browser.

+   - package.json: This adds all the necessary npm dependencies and creates a handful set of npm scripts to start building with Angular Universal:

```
"scripts": {
  "ng": "ng",
  "start": "ng serve",
  "build": "ng build",
  "watch": "ng build --watch --configuration development",
  "test": "ng test",
  "dev:ssr": "ng run gh-portfolio:serve-ssr",
  "serve:ssr": "node dist/gh-portfolio/server/main.js",
  "build:ssr": "ng build && ng run gh-portfolio:server",
  "prerender": "ng run gh-portfolio:prerender"
}
```