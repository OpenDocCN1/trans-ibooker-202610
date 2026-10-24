Java developers may also find convenient creating and controlling Tekton Pipelines and Tasks direcly from the code, using Fabric8 Tekton Java client. This option gives the full control from one single point, and you don’t need to maintain external manifests such as YAML files.

First import Maven dependency in POM file.

```
<dependencies>
    <dependency>
        <groupId>io.fabric8</groupId>
        <artifactId>tekton-client</artifactId>
        <version>${tekton-client.version}</version>
    </dependency>
</dependencies>
<properties>
    <tekton-client.version>4.12.0</tekton-client.version>
</properties>
```

Then you can use Tekton Java API to create Tasks or Pipeline:

```
package io.fabric8.tekton.api.examples;

import io.fabric8.tekton.client.*;
import io.fabric8.tekton.resource.v1alpha1.PipelineResource;
import io.fabric8.tekton.resource.v1alpha1.PipelineResourceBuilder;

public class PipelineResourceCreate {
    public static void main(String[] args) {
        try ( TektonClient client = ClientFactory.newClient(args) ) {
            String namespace = "coolstore";
            PipelineResource resource = new PipelineResourceBuilder()
                .withNewMetadata()
                .withName("client-repo")
                .endMetadata()
                .withNewSpec()
                .withType("git")
                .addNewParam()
                .withName("revision")
                .withValue("v4.2.2")
                .endParam()
                .addNewParam()
                .withName("url")
```