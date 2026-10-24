# Publishing a Package to Pub

Before you publish a package, you need to make sure you’ve filled in the package name, version, description, and any other information that you want to specify in `pubspec.yaml`, using Appendix 2, *Apple-Like Look and Additional App Configuration* as a reference.

A LICENSE file is also required.

If you don’t know what license to use, the Dart developers recommend the two-clause BSD license,[30] but there are many resources online to compare licenses, like choosealicense.com[31] by GitHub.

You should also create a README.md file, as this will be used as a home page for your package in Pub.

At this point, simply run:

```
pub publish
```

in the command line and, if you have done everything correctly, your package will be publicly available on Pub.