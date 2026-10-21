# Open Container Initiative (OCI) Specification

As mentioned earlier, containers were not a well-defined concept in the beginning. The Open Container Initiative (OCI) was established in 2015 to formalize the aforementioned runtime and image specifications.

A container’s runtime configuration is specified in a config.json file that is used by the container runtime to configure the container’s operational parameters. The use of an OCI-compliant container allows the same container image/configuration to be run on multiple container orchestrators (e.g., Docker and rkt) without the need to modify anything.

# OCI Image Specification

The OCI image specification outlines how the layers of the container are defined. Some of the properties of the image include:

+   - Author
+   - Architecture and operating system
+   - User/group to run the container as
+   - Ports that are exposed outside the container
+   - Environment variables
+   - Entry points and commands
+   - Working directory
+   - Image labels

The following is an example of a container image:

```
{
  "created": "2021-01-31T22:22:56.015925234Z",
  "author": "Michael Kehoe <michaelk@example.com>",
  "architecture": "x86_64",
  "os": "linux",
  "config": {

```