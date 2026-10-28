+   4. Run `npm run dev:ssr` to start the application and navigate to http://localhost:4200.

+   5. Use your browser to inspect the page, and you should see the following meta tags in the head element of the page:

```
<head>
  <meta charset="utf-8">
  <title>GitHub portfolio app</title>
  <base href="/">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
  <link rel="stylesheet" href="styles.css">
  <style>...</style>
  <style>...</style>
  <style>...</style>
  <style>...</style>
  <style>...</style>
  <meta name="description" content="<Your GitHub login>'s GitHub portfolio">
  <meta name="author" content="<Your GitHub login>">
```

Figure 7.8 – Application head element

Each of the popular social platforms, such as Twitter, Facebook, and LinkedIn, requires its own meta tags so that the URL of an SSR application can be correctly displayed on their platforms. If you do not want to add them manually using the Meta service of the Angular framework, you can use the ngx-seo library, which provides built-in methods for each platform. You can get the library from the npm registry at https://www.npmjs.com/package/ngx-seo.

In the following section, we will conclude our project by learning how to replay events not supported by an SSR application.

# Replaying events with preboot

In the Integrating Angular Universal section, we saw that an SSR application does not respond to user events other than navigations with the routerLink directive until it is entirely bootstrapped. We will now learn how to queue those events and replay them when the application has been fully loaded using the preboot library. Let's see how to use it:

+   1. Execute the following npm command to install the library:

```
npm install preboot
```