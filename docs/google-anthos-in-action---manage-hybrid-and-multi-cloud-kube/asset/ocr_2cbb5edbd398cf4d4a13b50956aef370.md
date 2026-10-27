![](bbox=[0.205, 0.095, 0.795, 0.52])

A typical Docker artifact is composed of two parts (see Figure 18.35). The first part is the M4A runtime, while the second part is the non-runnable base representing the capture system image layer from the migrated VM.

If you need to update the M4A runtime, then you can simply replace the first FROM directive as appropriate from the Docker file. For instance, suppose that you need to support M4A 1.8.1, then this can be easily achieved with the following new directive, which replaces the current one:

```
FROM anthos-migrate.gcr.io/v2k-run-embedded:v1.8.1 as migrate-for
```

If you need to update your application then you can change the second Docker FROM directive. In detail, you typically download the generated Docker file from your container registry, edit the Dockerfile to apply your desired changes, build a new layered image, and update the existing deployment with a rolling update. As discussed, this image layered approach