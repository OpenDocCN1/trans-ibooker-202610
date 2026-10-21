This command creates a very simple function that echos the CloudEvent that receives as input. If you take a look at the files generated inside the `fmtok8s-java-function` directory you will find a very typical Spring Boot Project:

![](bbox=[0.18, 0.2, 0.42, 0.45])

Figure 7.3 `func` scaffolded project

Next, let’s build our function

# 7.2.2 Building our function

Besides the `func.yaml` file there are no other YAML files or Dockerfiles in the project. Disregarding the lack of YAML files or Dockerfiles, we can still run the following command to create a container for this `func` project:

```
func build
```

You should see the following output:

```
A registry for Function images is required. For example, 'docker.io/tigerteam'.
? Registry for Function images: salaboy #1
Note: building a Function the first time will take longer than subsequent builds #2
Function image built: docker.io/salaboy/fmtok8s-java-function:latest #3
```

#1 `func` needs to know where to push the resulting image, if you have a Docker Hub account you can enter your user name here, and if you have already used `docker login` to authenticate with your account, it will use those credentials to upload the function container to your Docker Hub account.

#2 As `func` needs to fetch all the project dependencies, the first build will take a while, but subsequent builds will be faster, as `func` will catch these dependencies for future builds. The next time that you run `func build` it will check for changes and only download new dependencies if the `pom.xml` file has changed and recompile the source code, if there were any changes. The amount of time that this first build takes usually depends on your internet connection.

#3 Finally, the function is built, tagged and ready to use