```
image to tar file
JIB> Building image to tar file...
JIB> [====================] 80,0% complete > writing to tar file
JIB> [=======================] 100,0% complete
[INFO] k8s: ... modernizingjavaappsbook/catalog-spring-boot/1.0-SNAPSHOT/tmp/docker-build.tar,
successfully built
[INFO] ------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------
[INFO] Total time: 36.229 s
[INFO] Finished at: 2021-03-19T13:03:19+01:00
[INFO] ------------------------------------------------------------
```

JKube using Jib created the container image locally, and it is now ready to be pushed to Docker Hub. You can specify credentials in one of three ways:

+   - **Docker login**: You can login to your registry, in this case Dockerhub, and jKube will read ~/.docker/config.json file to get authentication details.
+   - **Provide credentials inside POM**: Provide registry credentials as part of XML configuration.
+   - **Provide credentials inside Maven Settings**: You can provide registry credentials in your ~/.m2/settings.xml file like this and plugin would read it from there.

In this case you use the third option and setup credentials into Maven Settings, so you can copy this file using your credentials. You also find the source code in this book’s GitHub repository.

```
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
```